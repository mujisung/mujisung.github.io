---
layout: posts
title: pytorch randint 의 size 부분에 tuple을 어떤 형식으로 넣어야 할까?
categories: [pytorch, ]
tag: [pytorch, ]  # tag가 추가됨.
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:** pytorch의 torch.randint에 대해서 알아보자.  
</div>


# 1. randint(end: int , size=(3,1): tuple) -> tensor\[\[1\],\[8\],\[9\]\])
```
import torch
tensor = torch.randint(10, (3,1))
tensor
```
하면 tensor\[\[1\],\[8\],\[9\]\]) 가 나온다.
0부터(start 생략하면 0부터) end까지의 정수 중에 uniform하게, 나오는 것.

# 2. randint(end: int , size=(3,): tuple) -> tensor\[1,8,9\])

# 3. randint(end: int , size=(3,0): tuple) -> tensor\[\])
아무것도 안나온다.

