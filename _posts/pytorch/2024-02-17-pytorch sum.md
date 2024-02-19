---
layout: posts
title: torch.sum 은 어떻게 써야할까?
categories: [pytorch, ]
tag: [pytorch, ]  # tag가 추가됨.
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:** torch.sum() 에 대해서 잘 알아보자!
</div>

# 1. import torch

# 2. x = torch.tensor(\[\[1,2,3\],\[4,5,6\]\])

# 3. torch.sum(x)
``` python
total_sum = torch.sum(x)
print(total_sum) # 21
```


# 4. torch.sum(x, dim=0)
``` python

col_sum = torch.sum(x, dim=0)
print(col_sum) # tensor([5,7,9])

```

# 5. torch.sum(x, dim=1)
``` python

row_sum = torch.sum(x, dim=1)
print(row_sum) # tensor([6,15])

```

# 6. torch.sum(x, dim=1, keepdim=True)

``` python

row_sum_keepdim = torch.sum(x, dim=1, keepdim=True)
print(row_sum_keepdim) # [[6], [15]]

```


일반화 하자면, dim=x라고 하면, x+1 번쨰 대괄호를 없애고, 그 안에 있는 sequence들끼리 평균을 낸다. 예를 들어서, 

``` python
x = torch.tensor([[[1,2],[3,4]],[[5,6],[7,8]]])
torch.sum(x, dim=0)
torch.sum(x, dim=1)
torch.sum(x, dim=2)

# 이 3개가 있다고 했을 때,
# [[[1,2],[3,4]],[[5,6],[7,8]]]

# 1. 여기서 dim=0 이라고 하면, 첫번쨰 대괄호를 빼면 [[1,2],[3,4]],[[5,6],[7,8]] 이렇게 된다. 이 두개의 element가 되는데, 이거를 그냥 평균을 내버린다. 그래서 결과는 [[ 6, 8], [10, 12]] 이게 된다.
# 2. dim=1 으로 하게 되면, [   [1,2],[3,4]    ]   [ [5,6],[7,8]     ]
# 3. dim=2 
```