---
layout: posts
title: prompt engineering
categories: [AI]
tag: [prompt engineering]
toc: true
toc_sticky: true
author_profile: false
sidebar:
  nav: "counts"
search: true
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:** what is prompt engineering? 
</div>







# GPT5 unlocks LLM System 2 Thinking?

영상: https://youtu.be/sD0X-lWPdxg?si=bctS5hdYZBFDRbx4

Daniel Kahneman: think fast and slow --> slow is conscious/logical/rational, fast is unconsious/automatic/intuitive

LLM은 fast thinking(intuition)이다. 인간은 근데 fast thinking과 slow thinking을, **1. time/accuracy tradeoff**를, **2. 언제 무엇에 집중할지를**, 자동적으로 계산하게 된다.

그렇다면 LLM이 slow thinking을 할 수 있게 하는 방법은 뭐가 있을까? **1. prompting(prompt engineering)** 과 **2. communicative agents**가 있다.
## prompt strategy


> [!1. chain of thoughts]  
> "Let's think step by step" 을 prompt 끝에 넣는다.

사실 LLM은 1+1이나, $$243*525^2$$나, 다 똑같이 다음 단어를 예측하고, sequence로 답을 도출하게 된다. 그래서 
이 방법론을 쓰면
