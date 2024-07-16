---
layout: posts
title: warming in neural networks
categories:
  - new_note
tags:
  - new_note
author_profile: false
sidebar:
  nav: counts
search: true
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:**  neural network에서 warming을 쓰는 이유를 알아보자
</div>


출처: [https://stackoverflow.com/a/55942518](https://stackoverflow.com/a/55942518)

결국 warming은, learning rate이 처음부터 크면, 초기 학습 데이터에 의해서 영향을 받게 된다. 그래서 learning rate을 초반에는 덜 주고, 후반에 많이 줌으로써, 잘 초기 데이터에 종속적이지 않게 만든다.

