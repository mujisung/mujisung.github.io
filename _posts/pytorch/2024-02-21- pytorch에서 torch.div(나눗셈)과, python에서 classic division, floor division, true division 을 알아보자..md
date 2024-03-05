---
layout: posts
title: pytorch에서 torch.div(나눗셈)과, python에서 classic division, floor division, true division 을 알아보자.
categories: [pytorch, ]
tag: [pytorch, python]  # tag가 추가됨.
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:**  pytorch에서 torch.div(나눗셈)과, python에서 classic division, floor division, true division 을 알아보자.
</div>


pytorch에서 division에 대해서 알아보다가, 

> By default, this performs a “true” division like Python 3. See the 'rounding_mode' argument for floor division.

이런 문장을 발견했다.

![](../../images/20240221-2024-02-21-%20classic%20division,%20floor%20division,%20true%20division%20in%20python-2.png)[^1]


# python의 division 종류

<br>
## floor division

내림(floor)를 수행하는 division인데, python에서는 // 가 floor division을 하는 연산자다.

c/c++에서는 그냥 나눗셈 operator인 `/` 이게 operand가 둘다 정수가 들어오면 정수 나눗셈을 하는데, 이거는 소수점을 0에 가까운 쪽으로 탈락시킨다. 무슨 말이냐면, -3.xxx는 -3으로, 3.xxx는 3으로 변환시킨다. 

과거 python2에서는, / 이 나눗셈 연산자가, operand가 둘다 정수일 때에는, flooring(내림)을 했다. 그래서 

**(x/-y) == -(x/y)** 이거를 수행하게 되면, False가 나왔다. ($$ \because $$ x가 15, y가 4일 때, (15/-4)는 -4가 되고, -(15/4)는 3이 되어서 그렇다.) [^2]


<br>



## true division


python3에서는 operand가 둘다 정수여도, float과 같은 연산을 수행하게 된다.

만약에 flooring을 수행하고 싶다면 // 연산자를 통해서 flooring을 할 수 있다.

<br>


## classic division [^2]

이거는 python과 관련없는 내용이다.

C/C++ 에서 사용되는 나눗셈 형태로, operand 두개가 둘다 정수이면, 정수값을 반환, 하나라도 float형이면 float값을 반환한다.


특히 C/C++에서는 값을 0에 가까운 쪽으로 정수 나눗셈을 하게 된다. -3.xxx도 -3, 3.xxx도 3으로 mapping한다.

<br>

# pytorch에서 division(/)

tensor / tensor 형태로, shape(차원)이 같은 tensor들끼리 나눗셈을 하게 되면 기본적으로 element-wise로 나눗셈을 진행하게 된다. 

근데 tensor1 / tensor2 형태로 나눗셈을 하게 되는데, shape(dimension)이 서로 다르면, **'pytorch의 broadcasting(브로드캐스팅)'** 을 통해서, 서로 다른 모양(shape)의 텐서가 연산을 수행할 수 있도록 한다.

한 텐서의 shape이 다른 tensor의 shape과 완전히 일치하는 경우, 또는 둘중 하나의 차원 크기가 1일 때, 즉 (4,1), (1,4) shape을 가지는 tensor가 있다면, 이게 (4,4) 차원의 tensor로 확장되어서(**broadcasting**), 요소별 연산을 수행하게 된다.





[^1]: [https://pytorch.org/docs/stable/generated/torch.div.html](https://pytorch.org/docs/stable/generated/torch.div.html)
[^2]: [https://5kyc1ad.tistory.com/291](https://5kyc1ad.tistory.com/291)

