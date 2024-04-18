---
layout: posts
title: #title
categories: # [coding, ]
tag: # [blog, ]  # tag가 추가됨.
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation: ** lecture 2,3. bellman optimality equation이랑 DP의 control iteration에 대해서 알아보자.  
</div>

강의영상:
[https://www.youtube.com/watch?v=PnHCvfgC_ZA](https://www.youtube.com/watch?v=PnHCvfgC_ZA)


![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-1.png)

최적 policy를 어떻게 찾는가. 

모든 policy에 대해서, 
a = max_x(f(x)) 라는 뜻은, for all x, f(x)의 최대값을 a에 저장한다.

즉 ![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-2.png) 해당 s, a일 때, 가장 높은 reward는 무엇인가. 

결국, q star를 알면, 끝난다. MDP

**결국에는 q star를 알면**, 어느 곳으로 갈지를 안다. 왜? reward가 80인 곳을 가지, 70인 곳을 가지는 않을 것이니깐.


# 식1,2, optimal svf, avf
![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-3.png)![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-2.png)

# 식3, optimal policy

![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-4.png)


# 식4,5,6,7 bellman optimality equation for v_*, q_* func
![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-6.png)
![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-5.png)

![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-7.png)
![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-8.png)

BEE는 평균으로 구할 수 있었는데,
BOE를 푸는 것은 불가능하다. 왜? non-linear equation이기 때문에. max와 expectation이 섞여있기 때문에, 그래서 no-closed form. 그래서 iterative solution methods. 가 있다.
![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-9.png)



# value iteration 이해 안되서 추가

그냥 전통적인 알고리즘이라면 왼쪾 위에서부터 오른쪾 아래까지 사선으로 숫자를 매겨주면 될텐데,  goal, loop, reward 떄문에, 모든 state에 대해서, 평가를 한다.



![](https://lh7-us.googleusercontent.com/DMJ5JbQOvED1aFNARrIcXa2E1L1EZ65qj_9rSI4VLQCuujJmNkeuIhNc38IKCxxNvkb1zSgfqJgcYnoVlKbO1Ja5_kIeU75O1gHRh8wxk_w9ABMCi_6-snz8Mj_vq2jSeHVeSZC8dcNDbvOU8BzxoCM)

  

결국에는 value iteration은 모든 state(여기서는 15개의 state)에서 평가를 한다. 1번에서는, 우선 다 0으로 시작하고, 1번에서, 내가 어떤 state를 가던, 다 -1이다. 그래서 다 -1을 넣어준다.

  

v1—>2 상황에서는, 우선 

  

123

4567

891011

12131415 라고 할 때,

1번부터 보면, 자기가 옆으로 가니깐, -1이 되네? 즉 ![](https://lh7-us.googleusercontent.com/47ss_vUGW0UNRGCOFUvbkZX2W6ENBNwzlOyexGa3hGH2XXQEO7ocxxlgQujKRG3wf09UESmt9FaGrIrBha2yZwYZupXC3zm-01Mxw119o0hwkdfdBFIro25qe6-7uChGeGWvq39d0rlW__zh22wWrl4) 이 식에 의해서, 가장 max를 시키는 action을 수행하는 action에 대해서, 그냥 그 action value를 값을 가지게 된다.

![](https://lh7-us.googleusercontent.com/LoQr-49LuZNosUHOF7MGJ0BSiu6iOt_5_ggxwD1T9BvKaNhMBMgsRndbMGCUEjhMglS4514hrQHriUmDQMUEEatDVQc115lBiahHBPLtryXORSQypGZcYlaqXxo23xMLhkfE3VFwBuD969k_HQNN3Tk)

optimal action value는 이렇게 되니깐. 그냥 다 더하는 것.

  

그래서, 그냥 -1로 고정되는 한편, 

  
  

2번 state에서는, 어디를 가던 다 -2가 된다. 그래서 -2로 update가 된다.

  
# state-value function으로 bellman optimality eq를 쓰는 이유는 복잡도 때문인듯.

![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-11.png)

![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-10.png)

state-action pair: mn
next-state-action pair mn
그래서 (mn)^2

![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-13.png)

DP를 설계할 수 있는 방법.

밑에있는 식의 오른ㄴ쪽 v(s') 은 new value를 그냥 바로 넣는거다. 이렇게 하면 새로운 정보가 더 많아서, 더 효율적이게 된다. 그러면 어떻게 효율적으로 compute하기 위해서 ordering을 할까? 어떤 문제에 대해서는, 굉장히 더욱 효율적이게 되는 것이다.


# new value function이 root,
# old value function이 leaves


**new value function이 root,**
**old value function이 leaves**


![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-14.png)

그래서 prioritize sweeping이라는 아이디어가 나온다.

이 아이디어는, mdp의 어떤 state의 update가 얼마나 중요한지에 대한 평가측도를 만드는 것이다.

그래서 어떤 것을 먼저 update해야 하는가? 그래서 priority queue를 줘서, 뭐가 더 중요한지 넣고, 그래서 그거 먼저 update하자다.

뭐가 더 중요한지를, bellman equation을 통해서 구한다. 

직관은, 더 많이 state가 변하는 것은, 즉 0에서 1000까지 마지막에는 변하면, 그걸로 ordering을 한다.



![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-15.png)

real time dynamic programming

select the state that agent actually visit.

모든 것을 sweep하지 않고, agent를 실제 세계에 놓는다.

실제 tragactory를 random sample해서, 그 real sample 주변에서 update한다.

![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-16.png)

DP는 full-width backup을 사용한다.

모든 action과 모든 state를 본다. 이거는 굉장히 비쌈. 

그리고 모든 tragectory를 보는 것이 아니라, sampliing을 할 것이다.

그래서 1 state도 compute하기가 어렵다. 백만개의 state가 있으면.

![](../../images/20240327-2024-03-27-RL,%20optimal%20value%20function-18.png)

state - action - one sample로 backup할 것이다.

curse of dimensionality를 해결한다. sampling을 통해서.

그리고, 환경의 dynamic을 sampling 하니깐, 환경의 모델을 몰라도 된다.