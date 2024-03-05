---
layout: posts
title: model이 brittle, spurious 하다는 것의 의미란 무엇일까?
categories: [AI, ]
tag: [brittle, spurious ]  # tag가 추가됨.
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:**  model이 brittle, spurious 하다는 것의 의미란 무엇일까?
</div>



출처: [NLP’s generalization problem, and how researchers are tackling it](https://thegradient.pub/frontiers-of-generalization-in-natural-language-processing/)


# brittle

## State-of-the-art NLP models are **brittle**

They fail when text is modified, even though its meaning is preserved:



## State-of-the-art NLP models are **spurious**

They often memorize artifacts and biases instead of truly learning:



출처: [https://re-code-cord.tistory.com/entry/Inductive-Bias%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C](https://re-code-cord.tistory.com/entry/Inductive-Bias%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C)

inductive bias 와 brittle, spurious

