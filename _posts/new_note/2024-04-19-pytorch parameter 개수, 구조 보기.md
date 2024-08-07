---
layout: posts
title: pytorch parameter 개수, 구조 보기#title
categories: pytorch
tag: pytorch
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:**  pytorch paramter 보는 방법 정리
</div>

<br>

``` python

  
  
def format_params(num_params):
	if num_params >= 1e9:
		return f"{num_params / 1e9:.2f} G"
	elif num_params >= 1e6:
		return f"{num_params / 1e6:.2f} M"
	elif num_params >= 1e3:
		return f"{num_params / 1e3:.2f} K"
	else:
		return str(num_params)

def params_in_mb(num_params):
	# Assuming the parameter type is float32, which takes 4 bytes
	return (num_params * 4) / (1024 * 1024)

  
# 메시지 로깅
print("\n\nnumber of parameters:")
total_params = sum(p.numel() for p in model.parameters())
trainable_params = sum(p.numel() for p in model.parameters() if p.requires_grad)
non_trainable_params = total_params - trainable_params

print(f'Total parameters: {total_params}')
print(f'Trainable parameters: {trainable_params}')
print(f'Non-trainable parameters: {non_trainable_params}')

print("\n\nnumber of parameters:")
print(f"Total parameters: {format_params(total_params)} ({params_in_mb(total_params):.3f} MB)")
print(f"Trainable parameters: {format_params(trainable_params)} ({params_in_mb(trainable_params):.3f} MB)")
print(f"Non-trainable parameters: {format_params(non_trainable_params)} ({params_in_mb(non_trainable_params):.3f} MB)")

```

이런 식으로 가능.

``` text

number of parameters:
Total parameters: 11952168
Trainable parameters: 11942568
Non-trainable parameters: 9600


number of parameters:
Total parameters: 11.95 M (45.594 MB)
Trainable parameters: 11.94 M (45.557 MB)
Non-trainable parameters: 9.60 K (0.037 MB)

```

그러면 결과가 이렇게 나옴.





# 진짜 양식대로 출력하는 방법.


## +model의 tensor dtype에 따라서 실제 MB로 보는 법(parameter 수가 아니라 Byte로 보는 것.)


``` python

def format_params(num_params):
    if num_params >= 1e9:
        return f"{num_params / 1e9:.2f} G"
    elif num_params >= 1e6:
        return f"{num_params / 1e6:.2f} M"
    elif num_params >= 1e3:
        return f"{num_params / 1e3:.2f} K"
    else:
        return str(num_params)

def dtype_to_bytes(dtype):
    if dtype in [torch.float32, torch.float]:
        return 4
    elif dtype in [torch.float64, torch.double]:
        return 8
    elif dtype in [torch.float16, torch.half]:
        return 2
    elif dtype in [torch.int32, torch.int]:
        return 4
    elif dtype in [torch.int64, torch.long]:
        return 8
    elif dtype in [torch.int16, torch.short]:
        return 2
    elif dtype == torch.uint8:
        return 1
    elif dtype == torch.bool:
        return 1 / 8  # This is a bit misleading as memory alignment usually prevents 1/8 byte usage
    else:
        return "Unknown"

def print_model_details(model):
    print("Model Details:")
    print("------------------------------------------------------------------------------------------------")
    total_params = 0
    total_bytes = 0
    for name, module in model.named_modules():
        if len(list(module.children())) == 0:  # Only print leaf modules
            params = sum(p.numel() for p in module.parameters())
            bytes_per_param = sum(dtype_to_bytes(p.dtype) * p.numel() for p in module.parameters())
            formatted_params = format_params(params)
            formatted_bytes_per_param = format_params(bytes_per_param)
            print(f"{name:30} | {str(module.__class__.__name__):15} | Params: {formatted_params} | Bytes: {formatted_bytes_per_param}")
            total_params += params
            total_bytes += bytes_per_param
    formatted_total_params = format_params(total_params)
    formatted_total_bytes = format_params(total_bytes)
    print("------------------------------------------------------------------------------------------------")
    print(f"Total Params: {formatted_total_params}, Total Bytes: {formatted_total_bytes} ")

# 모델 생성 및 요약 함수 호출
model = Resnet18(embedding_size=512, pretrained=False, is_norm=True, bn_freeze=False).cuda()
print_model_details(model)

```


## 결과:
``` text


Model Details:
------------------------------------------------------------------------------------------------
model.conv1                    | Conv2d          | Params: 9.41 K | Bytes: 37.63 K
model.bn1                      | BatchNorm2d     | Params: 128 | Bytes: 512
model.relu                     | ReLU            | Params: 0 | Bytes: 0
model.maxpool                  | MaxPool2d       | Params: 0 | Bytes: 0
model.layer1.0.conv1           | Conv2d          | Params: 36.86 K | Bytes: 147.46 K
model.layer1.0.bn1             | BatchNorm2d     | Params: 128 | Bytes: 512
model.layer1.0.relu            | ReLU            | Params: 0 | Bytes: 0
model.layer1.0.conv2           | Conv2d          | Params: 36.86 K | Bytes: 147.46 K
model.layer1.0.bn2             | BatchNorm2d     | Params: 128 | Bytes: 512
model.layer1.1.conv1           | Conv2d          | Params: 36.86 K | Bytes: 147.46 K
model.layer1.1.bn1             | BatchNorm2d     | Params: 128 | Bytes: 512
model.layer1.1.relu            | ReLU            | Params: 0 | Bytes: 0
model.layer1.1.conv2           | Conv2d          | Params: 36.86 K | Bytes: 147.46 K
model.layer1.1.bn2             | BatchNorm2d     | Params: 128 | Bytes: 512
model.layer2.0.conv1           | Conv2d          | Params: 73.73 K | Bytes: 294.91 K
model.layer2.0.bn1             | BatchNorm2d     | Params: 256 | Bytes: 1.02 K
model.layer2.0.relu            | ReLU            | Params: 0 | Bytes: 0
model.layer2.0.conv2           | Conv2d          | Params: 147.46 K | Bytes: 589.82 K
model.layer2.0.bn2             | BatchNorm2d     | Params: 256 | Bytes: 1.02 K
model.layer2.0.downsample.0    | Conv2d          | Params: 8.19 K | Bytes: 32.77 K
model.layer2.0.downsample.1    | BatchNorm2d     | Params: 256 | Bytes: 1.02 K
model.layer2.1.conv1           | Conv2d          | Params: 147.46 K | Bytes: 589.82 K
model.layer2.1.bn1             | BatchNorm2d     | Params: 256 | Bytes: 1.02 K
model.layer2.1.relu            | ReLU            | Params: 0 | Bytes: 0
model.layer2.1.conv2           | Conv2d          | Params: 147.46 K | Bytes: 589.82 K
model.layer2.1.bn2             | BatchNorm2d     | Params: 256 | Bytes: 1.02 K
model.layer3.0.conv1           | Conv2d          | Params: 294.91 K | Bytes: 1.18 M
model.layer3.0.bn1             | BatchNorm2d     | Params: 512 | Bytes: 2.05 K
model.layer3.0.relu            | ReLU            | Params: 0 | Bytes: 0
model.layer3.0.conv2           | Conv2d          | Params: 589.82 K | Bytes: 2.36 M
model.layer3.0.bn2             | BatchNorm2d     | Params: 512 | Bytes: 2.05 K
model.layer3.0.downsample.0    | Conv2d          | Params: 32.77 K | Bytes: 131.07 K
model.layer3.0.downsample.1    | BatchNorm2d     | Params: 512 | Bytes: 2.05 K
model.layer3.1.conv1           | Conv2d          | Params: 589.82 K | Bytes: 2.36 M
model.layer3.1.bn1             | BatchNorm2d     | Params: 512 | Bytes: 2.05 K
model.layer3.1.relu            | ReLU            | Params: 0 | Bytes: 0
model.layer3.1.conv2           | Conv2d          | Params: 589.82 K | Bytes: 2.36 M
model.layer3.1.bn2             | BatchNorm2d     | Params: 512 | Bytes: 2.05 K
model.layer4.0.conv1           | Conv2d          | Params: 1.18 M | Bytes: 4.72 M
model.layer4.0.bn1             | BatchNorm2d     | Params: 1.02 K | Bytes: 4.10 K
model.layer4.0.relu            | ReLU            | Params: 0 | Bytes: 0
model.layer4.0.conv2           | Conv2d          | Params: 2.36 M | Bytes: 9.44 M
model.layer4.0.bn2             | BatchNorm2d     | Params: 1.02 K | Bytes: 4.10 K
model.layer4.0.downsample.0    | Conv2d          | Params: 131.07 K | Bytes: 524.29 K
model.layer4.0.downsample.1    | BatchNorm2d     | Params: 1.02 K | Bytes: 4.10 K
model.layer4.1.conv1           | Conv2d          | Params: 2.36 M | Bytes: 9.44 M
model.layer4.1.bn1             | BatchNorm2d     | Params: 1.02 K | Bytes: 4.10 K
model.layer4.1.relu            | ReLU            | Params: 0 | Bytes: 0
model.layer4.1.conv2           | Conv2d          | Params: 2.36 M | Bytes: 9.44 M
model.layer4.1.bn2             | BatchNorm2d     | Params: 1.02 K | Bytes: 4.10 K
model.avgpool                  | AdaptiveAvgPool2d | Params: 0 | Bytes: 0
model.fc                       | Linear          | Params: 513.00 K | Bytes: 2.05 M
model.gap                      | AdaptiveAvgPool2d | Params: 0 | Bytes: 0
model.gmp                      | AdaptiveMaxPool2d | Params: 0 | Bytes: 0
model.embedding                | Linear          | Params: 262.66 K | Bytes: 1.05 M
------------------------------------------------------------------------------------------------
Total Params: 11.95 M

```

이렇게 나옴.



# 번외: torchinfo 깔아서 사용하는 것인데, 이거 비추천. 위에 껄로 그냥 하는게 나음.

pip install torchinfo



``` python

print("\n\nsummary 출력: ")
from torchinfo import summary
# 모델 요약 출력
summary(model, input_size=(1, 3, 224, 224))  # 적절한 입력 크기를 설정
print("summary 출력 끝 \n\n")
```



``` text
summary 출력: 
===============================================================================================
Layer (type:depth-idx)                        Output Shape              Param #
===============================================================================================
Resnet18                                      [1, 512]                  --
├─ResNet: 1-1                                 --                        513,000
│    └─Conv2d: 2-1                            [1, 64, 112, 112]         9,408
│    └─BatchNorm2d: 2-2                       [1, 64, 112, 112]         (128)
│    └─ReLU: 2-3                              [1, 64, 112, 112]         --
│    └─MaxPool2d: 2-4                         [1, 64, 56, 56]           --
│    └─Sequential: 2-5                        [1, 64, 56, 56]           --
│    │    └─BasicBlock: 3-1                   [1, 64, 56, 56]           73,984
│    │    └─BasicBlock: 3-2                   [1, 64, 56, 56]           73,984
│    └─Sequential: 2-6                        [1, 128, 28, 28]          --
│    │    └─BasicBlock: 3-3                   [1, 128, 28, 28]          230,144
│    │    └─BasicBlock: 3-4                   [1, 128, 28, 28]          295,424
│    └─Sequential: 2-7                        [1, 256, 14, 14]          --
│    │    └─BasicBlock: 3-5                   [1, 256, 14, 14]          919,040
│    │    └─BasicBlock: 3-6                   [1, 256, 14, 14]          1,180,672
│    └─Sequential: 2-8                        [1, 512, 7, 7]            --
│    │    └─BasicBlock: 3-7                   [1, 512, 7, 7]            3,673,088
│    │    └─BasicBlock: 3-8                   [1, 512, 7, 7]            4,720,640
│    └─AdaptiveAvgPool2d: 2-9                 [1, 512, 1, 1]            --
│    └─AdaptiveMaxPool2d: 2-10                [1, 512, 1, 1]            --
│    └─Linear: 2-11                           [1, 512]                  262,656
===============================================================================================
Total params: 11,952,168
Trainable params: 11,942,568
Non-trainable params: 9,600
Total mult-adds (G): 1.81
===============================================================================================
Input size (MB): 0.60
Forward/backward pass size (MB): 39.74
Params size (MB): 45.76
Estimated Total Size (MB): 86.10
===============================================================================================
summary 출력 끝 

```


# 최종 정리

## API

``` python

total_param_trainable_Non_trainable(model)  # 이거는 trainable, non-trainable parameter 개수 보여줌.

print_model_details(model) # 이거는 각 layer에 대한 정보 보여줌.

```


``` python

def total_param_trainable_Non_trainable(model):

	def format_params(num_params):
		if num_params >= 1e9:
			return f"{num_params / 1e9:.2f} G"
		elif num_params >= 1e6:
			return f"{num_params / 1e6:.2f} M"
		elif num_params >= 1e3:
			return f"{num_params / 1e3:.2f} K"
		else:
			return str(num_params)

	def params_in_mb(num_params):
		# Assuming the parameter type is float32, which takes 4 bytes
		return (num_params * 4) / (1024 * 1024)
	
	# 메시지 로깅
	print("\n\nnumber of parameters:")
	total_params = sum(p.numel() for p in model.parameters())
	trainable_params = sum(p.numel() for p in model.parameters() if p.requires_grad)
	non_trainable_params = total_params - trainable_params
	
	print(f'Total parameters: {total_params}')
	print(f'Trainable parameters: {trainable_params}')
	print(f'Non-trainable parameters: {non_trainable_params}')

	print("\n\nnumber of parameters:")
	print(f"Total parameters: {format_params(total_params)} ({params_in_mb(total_params):.3f} MB)")
	print(f"Trainable parameters: {format_params(trainable_params)} ({params_in_mb(trainable_params):.3f} MB)")
	print(f"Non-trainable parameters: {format_params(non_trainable_params)} ({params_in_mb(non_trainable_params):.3f} MB)")


```


``` python



def print_model_details(model):

    def format_params(num_params):
        if num_params >= 1e9:
            return f"{num_params / 1e9:.2f} G"
        elif num_params >= 1e6:
            return f"{num_params / 1e6:.2f} M"
        elif num_params >= 1e3:
            return f"{num_params / 1e3:.2f} K"
        else:
            return str(num_params)

    def dtype_to_bytes(dtype):
        if dtype in [torch.float32, torch.float]:
            return 4
        elif dtype in [torch.float64, torch.double]:
            return 8
        elif dtype in [torch.float16, torch.half]:
            return 2
        elif dtype in [torch.int32, torch.int]:
            return 4
        elif dtype in [torch.int64, torch.long]:
            return 8
        elif dtype in [torch.int16, torch.short]:
            return 2
        elif dtype == torch.uint8:
            return 1
        elif dtype == torch.bool:
            return 1 / 8  # This is a bit misleading as memory alignment usually prevents 1/8 byte usage
        else:
            return "Unknown"
    print("Model Details:")
    print("------------------------------------------------------------------------------------------------")
    for name, param in model.named_parameters():
	    print(name, param.shape)
	print()
	print()
	print()
	print()



    total_params = 0
    total_bytes = 0
    for name, module in model.named_modules():
        if len(list(module.children())) == 0:  # Only print leaf modules
            params = sum(p.numel() for p in module.parameters())
            requires_grad = any(p.requires_grad for p in module.parameters())
            bytes_per_param = sum(dtype_to_bytes(p.dtype) * p.numel() for p in module.parameters())
            formatted_params = format_params(params)
            formatted_bytes_per_param = format_params(bytes_per_param)
            print(f"{name:30} | {str(module.__class__.__name__):15} | Params: {formatted_params} | Bytes: {formatted_bytes_per_param} | requires_Grad: {requires_grad}")
            total_params += params
            total_bytes += bytes_per_param
    formatted_total_params = format_params(total_params)
    formatted_total_bytes = format_params(total_bytes)
    print("------------------------------------------------------------------------------------------------")
    print(f"Total Params: {formatted_total_params}, Total Bytes: {formatted_total_bytes} ")


```

### requires_grad = any(p.requires_grad for p in module.parameters()) 

이 부분은 parameter가 requires grad가 False인지, True인지 알 수 있게 해준다.



# 마지막 추상화

``` python

def summary(model):
	print("\n\nprint_model_summary 시작 \n\n")
	total_param_trainable_Non_trainable(model)
	print_model_details(model)
	print("\n\nprint_model_summary 끝\n\n")
	
```



## 더 추가


``` python

    for name, param in model.named_parameters():
        print(name, param.shape)
    print()
    print()
    print()
    print()
```

이렇게 하면 parameter.shape도 볼 수 있다.


``` python

import torch
import torch.nn as nn

# 예시 모델
class MyModel(nn.Module):
    def __init__(self):
        super(MyModel, self).__init__()
        self.conv1 = nn.Conv2d(1, 20, 5)
        self.conv2 = nn.Conv2d(20, 50, 5)
        self.fc1 = nn.Linear(800, 500)
        self.fc2 = nn.Linear(500, 10)

    def forward(self, x):
        x = torch.relu(self.conv1(x))
        x = torch.max_pool2d(x, 2, 2)
        x = torch.relu(self.conv2(x))
        x = torch.max_pool2d(x, 2, 2)
        x = x.view(-1, 800)
        x = torch.relu(self.fc1(x))
        x = self.fc2(x)
        return x

# 모델 인스턴스 생성
model = MyModel()

def print_model_parameters(model):
    print("{:<85} | {:<20} | {:<10}".format("Layer", "Type", "Parameters"))
    print("="*120)
    total_params = 0
    for name, module in model.named_modules():
        # 모듈 내 파라미터의 수 계산
        num_params = sum(p.numel() for p in module.parameters())
        if num_params > 0:  # 파라미터가 있는 층만 출력
            print("{:<85} | {:<20} | {:<10}".format(name, type(module).__name__, num_params))
            total_params += num_params
    print("="*120)
    print("Total Parameters:", total_params)

print_model_parameters(model)


```