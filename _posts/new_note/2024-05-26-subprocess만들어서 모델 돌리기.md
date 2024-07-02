---
layout: posts
title: subprocess만들어서 모델 돌리기
categories: [new_note, ]
tag: [new_note, ]  # tag가 추가됨.
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:**  subprocess만들어서 모델 돌리기
</div>




``` python
# Copyright (c) 2021, NVIDIA CORPORATION.  All rights reserved.
#
# NVIDIA CORPORATION and its licensors retain all intellectual property
# and proprietary rights in and to this software, related documentation
# and any modifications thereto.  Any use, reproduction, disclosure or
# distribution of this software and related documentation without an express
# license agreement from NVIDIA CORPORATION is strictly prohibited.

"""Project given image to the latent space of pretrained network pickle."""

import copy
import os
from time import perf_counter

import click
import imageio
import numpy as np
import PIL.Image
import torch
import torch.nn.functional as F

import dnnlib
import legacy

import multiprocessing
from tqdm import tqdm
from itertools import chain

def project(
    G,
    target: torch.Tensor,  # d[C,H,W] and dynamic range [0,255], W & H must match G output resolution
    *,
    num_steps                  = 150,
    w_avg_samples              = 10000,
    initial_learning_rate      = 0.1,
    initial_noise_factor       = 0.05,
    lr_rampdown_length         = 0.25,
    lr_rampup_length           = 0.05,
    noise_ramp_length          = 0.75,
    regularize_noise_weight    = 1e5,
    verbose                    = False,
    device: torch.device
):
    assert target.shape == (G.img_channels, G.img_resolution, G.img_resolution)

    def logprint(*args):
        if verbose:
            print(*args)

    G = copy.deepcopy(G).eval().requires_grad_(False).to(device) # type: ignore

    # Compute w stats.
    logprint(f'Computing W midpoint and stddev using {w_avg_samples} samples...')
    z_samples = np.random.RandomState(123).randn(w_avg_samples, G.z_dim)
    w_samples = G.mapping(torch.from_numpy(z_samples).to(device), None)
    w_samples = w_samples[:, :1, :].cpu().numpy().astype(np.float32)
    w_avg = np.mean(w_samples, axis=0, keepdims=True)
    w_std = (np.sum((w_samples - w_avg) ** 2) / w_avg_samples) ** 0.5

    # Setup noise inputs.
    noise_bufs = { name: buf for (name, buf) in G.synthesis.named_buffers() if 'noise_const' in name }

    # Load VGG16 feature detector.
    url = 'https://nvlabs-fi-cdn.nvidia.com/stylegan2-ada-pytorch/pretrained/metrics/vgg16.pt'
    with dnnlib.util.open_url(url) as f:
        vgg16 = torch.jit.load(f).eval().to(device)

    # Features for target image.
    target_images = target.unsqueeze(0).to(device).to(torch.float32)
    if target_images.shape[2] > 256:
        target_images = F.interpolate(target_images, size=(256, 256), mode='area')
    target_features = vgg16(target_images, resize_images=False, return_lpips=True)

    w_opt = torch.tensor(w_avg, dtype=torch.float32, device=device, requires_grad=True)
    optimizer = torch.optim.Adam([w_opt] + list(noise_bufs.values()), betas=(0.9, 0.999), lr=initial_learning_rate)

    # Init noise.
    for buf in noise_bufs.values():
        buf[:] = torch.randn_like(buf)
        buf.requires_grad = True

    for step in range(num_steps):
        # Learning rate schedule.
        t = step / num_steps
        w_noise_scale = w_std * initial_noise_factor * max(0.0, 1.0 - t / noise_ramp_length) ** 2
        lr_ramp = min(1.0, (1.0 - t) / lr_rampdown_length)
        lr_ramp = 0.5 - 0.5 * np.cos(lr_ramp * np.pi)
        lr_ramp = lr_ramp * min(1.0, t / lr_rampup_length)
        lr = initial_learning_rate * lr_ramp
        for param_group in optimizer.param_groups:
            param_group['lr'] = lr

        # Synth images from opt_w.
        w_noise = torch.randn_like(w_opt) * w_noise_scale
        ws = (w_opt + w_noise).repeat([1, G.mapping.num_ws, 1])
        synth_images = G.synthesis(ws, noise_mode='const')

        # Downsample image to 256x256 if it's larger than that.
        synth_images = (synth_images + 1) * (255/2)
        if synth_images.shape[2] > 256:
            synth_images = F.interpolate(synth_images, size=(256, 256), mode='area')

        # Features for synth images.
        synth_features = vgg16(synth_images, resize_images=False, return_lpips=True)
        dist = (target_features - synth_features).square().sum()

        # Noise regularization.
        reg_loss = 0.0
        for v in noise_bufs.values():
            noise = v[None,None,:,:]
            while True:
                reg_loss += (noise*torch.roll(noise, shifts=1, dims=3)).mean()**2
                reg_loss += (noise*torch.roll(noise, shifts=1, dims=2)).mean()**2
                if noise.shape[2] <= 8:
                    break
                noise = F.avg_pool2d(noise, kernel_size=2)
        loss = dist + reg_loss * regularize_noise_weight

        # Step
        optimizer.zero_grad(set_to_none=True)
        loss.backward()
        optimizer.step()
        # logprint(f'step {step+1:>4d}/{num_steps}: dist {dist:<4.2f} loss {float(loss):<5.2f}')
        # Normalize noise.
        with torch.no_grad():
            for buf in noise_bufs.values():
                buf -= buf.mean()
                buf *= buf.square().mean().rsqrt()

    # Return single w_opt as [1, 1, 512] for all layers.
    return w_opt.detach()

#----------------------------------------------------------------------------

def process_images(subdir_list, network_pkl, device, num_steps):
    # 각 프로세스에서 네트워크를 별도로 로드
    with dnnlib.util.open_url(network_pkl) as fp:
        G = legacy.load_network_pkl(fp)['G_ema'].requires_grad_(False).to(device)
    
    for subdir in subdir_list:
        for filename in os.listdir(subdir):
            if filename.lower().endswith('.jpg'):
                file_path = os.path.join(subdir, filename)
                target_pil = PIL.Image.open(file_path).convert('RGB')
                w, h = target_pil.size
                s = min(w, h)
                target_pil = target_pil.crop(((w - s) // 2, (h - s) // 2, (w + s) // 2, (h + s) // 2))
                target_pil = target_pil.resize((G.img_resolution, G.img_resolution), PIL.Image.LANCZOS)
                target_uint8 = np.array(target_pil, dtype=np.uint8)
                
                projected_w = project(
                    G,
                    target=torch.tensor(target_uint8.transpose([2, 0, 1]), device=device),
                    num_steps=num_steps,
                    device=device,
                    verbose=False
                )
                
                npz_out_path = os.path.join(subdir, os.path.splitext(filename)[0] + '.npz')
                np.savez(npz_out_path, w=projected_w.cpu().numpy())
                print(f"Saved {npz_out_path}")

# 메인 함수 및 멀티프로세싱 설정은 동일하게 유지


@click.command()
@click.option('--network', 'network_pkl', required=True, help='Network pickle filename')
@click.option('--target-dir', 'target_dir', required=True, help='Directory containing target images', metavar='DIR')
@click.option('--num-steps', default=150, help='Number of optimization steps', show_default=True, type=int)
@click.option('--num-processes', default=8, help='Number of parallel processes', show_default=True, type=int)
def run_projection(network_pkl, target_dir, num_steps, num_processes):
    device = torch.device('cuda')
    subdir_list = [os.path.join(target_dir, d) for d in os.listdir(target_dir) if os.path.isdir(os.path.join(target_dir, d))]
    chunks = [subdir_list[i::num_processes] for i in range(num_processes)]
    
    processes = []
    for chunk in chunks:
        p = multiprocessing.Process(target=process_images, args=(chunk, network_pkl, device, num_steps))
        processes.append(p)
        p.start()

    for p in processes:
        p.join()



if __name__ == "__main__":
    run_projection() # pylint: disable=no-value-for-parameter
    
#----------------------------------------------------------------------------

```



각 subprocess마다 G network을 받아오도록 설계했다.

# 질문: 근데 보통 multi gpu 환경에서 subprocess를 만들어서 사용하진 않지?

ChatGPT

맞습니다. 일반적으로 멀티 GPU 환경에서는 서브프로세스를 사용하는 것보다 PyTorch의 기본 제공 기능인 DataParallel 또는 DistributedDataParallel (DDP)을 사용하는 것이 더 일반적이고 효율적입니다. 이러한 방법은 GPU 간의 통신을 최적화하고 프로그램의 복잡성을 줄여줍니다.

### DataParallel

`torch.nn.DataParallel`은 싱글 머신 내의 여러 GPU에 모델을 복제하여 작업을 분배하고, 결과를 자동으로 합칩니다. 이 방식은 간단한 사용법으로 인기가 있지만, 각 GPU 간의 데이터 복사로 인해 오버헤드가 크고, 배치 사이즈가 커질수록 스케일링 효율이 떨어지는 단점이 있습니다.

### DistributedDataParallel (DDP)

`torch.nn.parallel.DistributedDataParallel`은 멀티프로세스를 사용하여 각 프로세스가 하나의 GPU를 독립적으로 사용하게 합니다. DDP는 네트워크 상에서 데이터를 효율적으로 분배하고, 각 GPU에서 독립적으로 연산을 수행 후 결과를 합치는 과정을 최적화합니다. 이는 대규모 분산 학습 환경에서 더 나은 성능과 확장성을 제공합니다.

### 코드 예시: DistributedDataParallel 사용

멀티 GPU 환경에서 이미지 프로젝션 작업을 DDP로 구현하는 간단한 예제를 제공하겠습니다. 이 예제는 각 GPU에서 독립적으로 모델을 로드하고 연산을 수행하는 방식을 보여줍니다.

python
``` python

import torch
import torch.distributed as dist
import torch.multiprocessing as mp
from torch.nn.parallel import DistributedDataParallel as DDP

def setup(rank, world_size):
    dist.init_process_group("nccl", rank=rank, world_size=world_size)

def cleanup():
    dist.destroy_process_group()

def project_ddp(rank, world_size, network_pkl, target_dir, num_steps):
    setup(rank, world_size)
    device = torch.device(f'cuda:{rank}')
    
    # 모델과 기타 필요 자원을 device에 로드
    # 예: G = load_model(network_pkl).to(device)
    # DDP로 모델을 래핑
    # G = DDP(G, device_ids=[rank])

    # 이미지 프로젝션 로직 구현
    # 프로세스 종료 후 cleanup 호출
    cleanup()

def run_projection_ddp(network_pkl, target_dir, num_steps, num_gpus):
    mp.spawn(project_ddp,
             args=(num_gpus, network_pkl, target_dir, num_steps),
             nprocs=num_gpus,
             join=True)

if __name__ == "__main__":
    network_pkl = 'path_to_network.pkl'
    target_dir = 'path_to_images'
    num_steps = 150
    num_gpus = torch.cuda.device_count()
    run_projection_ddp(network_pkl, target_dir, num_steps, num_gpus)

```

이 코드는 각 GPU에서 독립적으로 작업을 수행하도록 설정하며, DDP를 사용하여 각 GPU의 결과를 효율적으로 결합합니다. 이 방식은 복잡한 네트워크 통신을 최적화하고, 멀티 GPU 환경에서 더 높은 성능을 제공합니다.

