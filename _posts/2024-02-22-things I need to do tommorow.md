---
layout: posts
title: #title
categories: # [coding, ]
tag: # [blog, ]  # tag가 추가됨.
toc: true   # 글 오른쪽에 toc가 나온다.
toc_sticky: true
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:** 내일 할일 정리  
</div>


주말까지, failure case 실제로 한거하고, 실제로 성능 올려와라. 한 5%, 3%. 성능 올려와. 나머지. failure case가 줄어들어서, 성능이 올랐다. 가정이 있으면 안된다. data, 수치적으로.

내가 해야할 것.

1. failure case를 확인하는 API 개발
2. 성능 올리기


1. failure case: 실제로 어떻게 뭐가 잘못 분류되었는지 확인하는 방법을 어떻게 만들어야 할까?

2. 성능 올리기는, torch.manual_seed(0)을 통해서, 고정시켜보자. reproducibility를 키우는 것.



2024-02-22 건희형 피드백:
``` text

진짜로 모양으로 분류하는지 봐야하고,
MOE로 근거

근거가 없이 얘기하고 있고, 설득이 되지 않는다.

resnet을 써서, 실제로 찎히는지 보고, 비율을 한번 봐봐라. 
cub에서 비율, 다른 dataset에서의 비율을 봐라. 
pose를 써서, 하는 비율을 보고, MOE를 해서 구별하는지를 봐라. 

randomness를 통제한 것이다. 원래 코드를 고정을 안시켰을 것이다.
pose를 어떻게 해결할까.

1. 일반적으로 나타나는 문제인지 봐야한다. failure case구나. 이렇게 안되는 경우도 있어요. 전체 sample dataset에서, 잘못맞춘 비율. pose가 같아서, 유사하게 되는지를, visualization.

CNN을 pose로 보고 규별을 하지 않는다. pose로 보고 구별을 할 수가 없다.

73% 정도가, 단순히 high level knowledge를 봤다는 것인데, 여기서 어폐가 안맞는다. pose 정보가 구별하는데 잘 쓰였다고 봐야한다.

가정 없이 이야기해야한다.

1. resnet 모델 파악해라. CNN 기반 모델들이 어떤 메커니즘이 어떻게 정보를 학습하는지 판단하고,
2. failure case가 어떻게 되는지. 봐라. 그래서 진짜로 이런 문제가 있다는 것을 보여줘야 한다. 그리고 나서 그런 문제를, MOE를 쓸 것이면, 이 문제를 해결하는데, 어떻게 해볼 것인가. 이렇게 해서 성능을 올려.

3. 주말까지, failure case 실제로 한거하고, 실제로 성능 올려와라. 한 5%, 3%. 성능 올려와. 나머지. failure case가 줄어들어서, 성능이 올랐다. 가정이 있으면 안된다. data, 수치적으로.

dataset 두개로 하는게 좋아보인다. 

두개만


```


