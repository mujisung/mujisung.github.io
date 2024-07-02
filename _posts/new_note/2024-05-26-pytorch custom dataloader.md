---
layout: posts
title: pytorch custom dataloader
categories: [new_note, ]
tag: [new_note, ]  # tag가 추가됨.
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:**  pytorch custom dataloader 사용법 정리
</div>

우선 코드는 다음과 같이 되어있다.
``` python

import torch
import torchvision
import numpy as np
import PIL.Image

class BaseDataset(torch.utils.data.Dataset):
    def __init__(self, root, mode, transform = None):
        self.root = root
        self.mode = mode
        self.transform = transform
        self.ys, self.im_paths, self.I = [], [], []

    def nb_classes(self):
        assert set(self.ys) == set(self.classes)
        return len(self.classes)

    def __len__(self):
        return len(self.ys)

    def __getitem__(self, index):
        def img_load(index):
            im = PIL.Image.open(self.im_paths[index])
            # convert gray to rgb
            if len(list(im.split())) == 1 : im = im.convert('RGB') 
            if self.transform is not None:
                im = self.transform(im)
            return im

        im = img_load(index)
        target = self.ys[index]

        return im, target, index

    def get_label(self, index):
        return self.ys[index]

    def set_subset(self, I):
        self.ys = [self.ys[i] for i in I]
        self.I = [self.I[i] for i in I]
        self.im_paths = [self.im_paths[i] for i in I]

import os
class CUBirds(BaseDataset):
    def __init__(self, root, mode, transform=None):
        self.root = root
        self.mode = mode
        self.transform = transform

        self.path_train_o = self.root + '/train_o'
        self.path_train_n_1 = self.root + '/train_n_1'
        self.path_eval_o = self.root + '/valid_o'
        self.path_eval_n_1 = self.root + '/valid_n_1'

        if self.mode == 'train_0':
            self.classes = range(0, 160)
            self.path = self.path_train_o

        elif self.mode == 'train_1':
            # self.classes = range(0, 200)
            self.path = self.path_train_n_1

        elif self.mode == 'eval_0':
            self.classes = range(0, 160)
            self.path = self.path_eval_o

        elif self.mode == 'eval_1':
            self.classes = range(0, 200)
            self.path = self.path_eval_n_1

        BaseDataset.__init__(self, self.path, self.mode, self.transform)

        index = 0
        for i in datasets.ImageFolder(root=self.path).imgs:
            # i[1]: label, i[0]: the full path to an image
            y = i[1]
            # fn needed for removing non-images starting with `._`
            fn = os.path.split(i[0])[1]
            self.ys += [y]
            self.I += [index]
            self.im_paths.append(i[0])
            index += 1

def load(name, root, mode, transform=None):
    CUBirds(root=root, mode=mode, transform=transform) # _type[name](root=root, mode=mode, transform=transform)

from torchvision import transforms

def make_transform(is_train = True):
    # Resolution Resize List : 256, 292, 361, 512
    # Resolution Crop List: 224, 256, 324, 448
    
    resnet_sz_resize = 256
    resnet_sz_crop = 224 
    resnet_mean = [0.485, 0.456, 0.406]
    resnet_std = [0.229, 0.224, 0.225]
    resnet_transform = transforms.Compose([
        transforms.RandomResizedCrop(resnet_sz_crop) if is_train else Identity(),
        transforms.RandomHorizontalFlip() if is_train else Identity(),
        transforms.Resize(resnet_sz_resize) if not is_train else Identity(),
        transforms.CenterCrop(resnet_sz_crop) if not is_train else Identity(),
        transforms.ToTensor(),
        transforms.Normalize(mean=resnet_mean, std=resnet_std)
    ])
    
    return resnet_transform


#### Dataset Loader and Sampler
dset_tr_0 = dataset.load(name=args.dataset, root=pth_dataset, mode='train_0', transform=dataset.utils.make_transform(is_train=True))
dlod_tr_0 = torch.utils.data.DataLoader(dset_tr_0, batch_size=args.sz_batch, shuffle=True, num_workers=args.nb_workers)
nb_classes = dset_tr_0.nb_classes()
```


이런 상황에서, 

## API
``` python
#### Dataset Loader and Sampler
dset_tr_0 = dataset.load(root=pth_dataset, mode='train_0', transform=make_transform(is_train=True))
dlod_tr_0 = torch.utils.data.DataLoader(dset_tr_0, batch_size=args.sz_batch, shuffle=True, num_workers=args.nb_workers)
nb_classes = dset_tr_0.nb_classes()
```

결국  `custom dataset`을 정의하고, torch.utils.data.DataLoader의 첫번째 인자로 `custom dataset`을 넣어주면 되는 것이다.

custom dataset은 어떻게 만들까?

\_\_len\_\_(self), \_\_getitem\_\_(self, index), \_\_init\_\_(self)는 반드시 있어야 한다고 한다. 

그리고 코드에서, I는 `custom dataset`의 index를 의미한다. 이 index는 \_\_getitem\_\_ 의 index로, `custom dataset의 index`다. 나중에 dataloader에서 shuffle=True로 해놓아서 이 index도 뒤죽박죽 될 수도 있는데, 어쩃든 I는 custom dataset의 index다.




``` python

      def __getitem__(self, index):
        def img_load(index):
            im = PIL.Image.open(self.im_paths[index])
            # convert gray to rgb
            if len(list(im.split())) == 1 : im = im.convert('RGB')
            if self.transform is not None:
                im = self.transform(im)
            return im
  
        im = img_load(index)
        target = self.ys[index]
        # 이미지 파일 경로에서 latent vector 파일 경로 생성
        latent_vector_path = f'{os.path.splitext(self.im_paths[index])[0]}.npz'
        if os.path.exists(latent_vector_path):
            latent_vector = np.load(latent_vector_path)['w']
            latent_vector = torch.tensor(latent_vector, dtype=torch.float32, device='cuda')  # GPU에 텐서를 할당
        else:
            # Latent vector 파일이 없다면 적절한 예외 처리 또는 로깅
            latent_vector = None  # 혹은 초기값 설정

        return im, target, index, latent_vector
```

그리고 latent_vector가 return이 될 때에는 tensor로 return이 되지 않으니깐, tensor로 바꾸어 주어야 한다. 

사실 그리고 device-'cuda'로 이렇게는 안해도 되고, dataloader에서 나온 것을 .to("cuda") 이렇게 해도 된다. 이렇게 해도 되는 이유는, 알아서 pytorch가 최적화를 해줘서 사실 성능 차이는 없다고 한다.

