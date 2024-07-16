---
layout: posts
title: information theory chapter 1
categories:  [information theory, ]
tag: [information theory, ]  
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:**  information theory 1장,
</div>

책은 [Gregory Falkovich](https://www.weizmann.ac.il/complex/falkovich/ "Home") 의 [book draft](https://www.weizmann.ac.il/complex/falkovich/sites/complex.falkovich/files/uploads/DraftPUPrev.pdf)로 하고, 강의 영상도 youtube에 있는데, 우선 책으로만 볼 것 같다.

총 191장의 책으로, 내용이 엄청 많은 책은 아닌 것 같다.

chapter는 총 8장으로, 엄청 많지는 않다. 

![](../../images/20240708-2024-07-08-information%20theory%201장-1.png)


# 1. Thermodynamics and statistical physics

이 chapter에서는, 'observable manifestations of the hidden degrees of freedom'에 대해서 다룬다. state, nature, DoF, spin, 등등을 모르는 상태에서, 시스템의 대칭과 보존에 대해서 알고 있다는 뜻이다.


## 1.1 Basics of thermodynamics

1824년 카르노는, 어떤 저수지에서, 줄어든 열만큼, 일을 하게 된다고 말함. 그리고 그 열은 온도에 비례해서, 일로 쓸 수 있는 최대치를 계산할 수 있게 된다. 
![](../../images/20240708-2024-07-08-information%20theory%201장-2.png)

1864년 클라시우스는 Q/T를 entropy라고 명명한다. 이 `엔트로피` 개념을 통해 이전의 카르노 예를 해석하자면, 

$$ \Delta{S_1} = \frac{Q_1}{T_1} $$
hot reservoir의 entropy 감소가, 
$$ \Delta{S_2} = \frac{Q_2}{T_2} $$
cold reservoir의 entropy 증가와 같아야 한다.


<br>

열역학에서 보존되는 개념은 E, energy다. 이 에너지는 observable variable의 static 값들로 규정이 된다.


state에서 state로 갈 때, energy change가 수반이 된다. 근데 그 energy change는 두개로 구성이 된다. **1. Work: energy change of visible degrees of freedom**  **2. Heat: energy change of hidden degrees of freedom**  

energy change를 측정하기 위해서는, 열 교환이 없는 adiabatic(단열) 과정이 필요하다. 

단열과정에서, energy change는 
$$ dE = \delta{Q} - \delta{W} $$ 로 표현된다.

이것은 열역학 제 1법칙으로, 에너지 보존 법칙을 의미한다. energy는 state의 함수여서 미분 기호를 쓰고, heat, work은 `delta` 기호를 쓴다. 왜 이런 기호를 쓰냐면, **Heat exchange와 work은 A에서 B로 갈 때 path에 의존하기 때문이다.** 

여기서 round는 variantional의 표현이다.[https://physics.stackexchange.com/a/65727](https://physics.stackexchange.com/a/65727)

