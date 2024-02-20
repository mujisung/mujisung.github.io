---
layout: posts
title: sliding window attention, KV-cache에 대해서 알아보자.
categories: [AI, ]
tag: [sliding window, KV cache ]  # tag가 추가됨.
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:**  sliding window attention, KV-cache에 대해서 알아보자.
</div>



sliding window의 크기를 3이라고 하자. 

embedding vector의 차원을 4096 이라고 하고, dim = d_model= 4096

attention head(multi-head attention)의 개수를 32개라고 하자. n_heads = h = 32.

그리고 token의 개수가 10개(입력문장의 개수)라고 하자.

그러면 d_k = d_v = d_model / n_heads = 128

그러면 
10 X 4096 행렬이 처음 Q가 되고, Q' = Q W_Q 를 진행하고,    (W_Q는 4096 * 128 행렬)

Q' X (K'^T) / sqrt(128) 를 각 행에 대해서 softmax를 하면, 유사도가 나온다. 그거를 V' 와 곱하면, attention이 나오는데, 그러면 출력이 10 * 128 행렬이 나오게 된다. 

실제로 W_Q, W_K, W_V 행렬은 4096 * 4096 행렬이다. 근데 이거를 32개의 4096 * 128 행렬을 열로 쌓아두게 되면, 4096 * 4096 행렬이 되는데, 실제로는 행렬을 이렇게 만들어서 계산을 하게 된다.

그러면 multi-head attention을 통해서, 10 * 4096 차원의 행렬이 나오게 된다. 이거를 다시 4096 * 4096 차원의 행렬인 W_O를 통해서, dense layer를 통과하게 되면, 최종 multihead attention이 나오게 된다.

즉 결국 transformer는, 10 * 4096 차원이 계속 유지가 된다.

근데 decoder base model과 encoder base model의 차이가 뭐냐면, 결국 tranformer block(multi-head attention + add&norm + feed forward + add&norm)을 N개 쌓으면, 결국 차원이 10 * 4096 이게 된다. 

근데 이거를 





출처:

1. [https://www.youtube.com/watch?v=UiX8K-xBUpE](https://www.youtube.com/watch?v=UiX8K-xBUpE)
2. [https://www.youtube.com/watch?v=Mn_9W1nCFLo](https://www.youtube.com/watch?v=Mn_9W1nCFLo)

