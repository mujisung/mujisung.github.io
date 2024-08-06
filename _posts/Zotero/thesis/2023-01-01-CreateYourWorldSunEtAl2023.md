---
layout: posts
title: # Proxy Anchor-based Unsupervised Learning for Continuous Generalized Category Discovery
categories: [thesis, AI]
tag: [thesis_name  ]  
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:**  
</div>

# self notes

### Research

* Methodology::
* Key Contructs::
	* IVs::
	* DVs::
	* Moderators::
	* Others::
* Key_Findings::
* Contributions::
* Limitations::

### Self Critique

* Critique
	* Strengths
	* Limitations

* How is it relevant to my research?
	* Relevant_topic::
	* Use::


> [!Cite]
> [1]  G. Sun, W. Liang, J. Dong, J. Li, Z. Ding와/과Y. Cong, “Create Your World: Lifelong Text-to-Image Diffusion”, 2023년 9월 8일, _arXiv_: arXiv:2309.04430. 접근된: 2024년 7월 25일. [Online]. Available at: [http://arxiv.org/abs/2309.04430](http://arxiv.org/abs/2309.04430)

> [!Synth]
> **Contribution::**

> [!Md]
>
> **Author**:: Sun, Gan> 
>
> **Author**:: Liang, Wenqi> 
>
> **Author**:: Dong, Jiahua> 
>
> **Author**:: Li, Jun> 
>
> **Author**:: Ding, Zhengming> 
>
> **Author**:: Cong, Yang> 
>
> **Title**:: Create Your World: Lifelong Text-to-Image Diffusion
> **Year**:: 2023
> **Citekey**:: [[@2023-01-01-CreateYourWorldSunEtAl2023]]
> 
> >**Tags**:: Computer Science - Artificial Intelligence, Computer Science - Computer Vision and Pattern Recognition
> >**itemType**:: preprint
> >
> >
> >
> >
> >
> >
> >
> >
> >

> [!Link]
> [Sun 등 - 2023 - Create Your World Lifelong Text-to-Image Diffusio.pdf](C:\Users\wys00\Zotero\storage\WU6N7PKX\Sun%20등%20-%202023%20-%20Create%20Your%20World%20Lifelong%20Text-to-Image%20Diffusio.pdf)

> [!Abstract]
>
> abstract:: Text-to-image generative models can produce diverse high-quality images of concepts with a text prompt, which have demonstrated excellent ability in image generation, image translation, etc. We in this work study the problem of synthesizing instantiations of a user’s own concepts in a never-ending manner, i.e., create your world, where the new concepts from user are quickly learned with a few examples. To achieve this goal, we propose a Lifelong text-to-image Diffusion Model (L2DM), which intends to overcome knowledge “catastrophic forgetting” for the past encountered concepts, and semantic “catastrophic neglecting” for one or more concepts in the text prompt. In respect of knowledge “catastrophic forgetting”, our L2DM framework devises a task-aware memory enhancement module and a elastic-concept distillation module, which could respectively safeguard the knowledge of both prior concepts and each past personalized concept. When generating images with a user text prompt, the solution to semantic “catastrophic neglecting” is that a concept attention artist module can alleviate the semantic neglecting from concept aspect, and an orthogonal attention module can reduce the semantic binding from attribute aspect. To the end, our model can generate more faithful image across a range of continual text prompts in terms of both qualitative and quantitative metrics, when comparing with the related state-of-the-art models. The code will be released at https://wenqiliang.github.io/. Random samples Index Terms—Lifelong Machine Learning, Stable Diffusion, Image Generation, Continual Learning. 
>

---

# Annotations 


![](images_for_zotero/2023-01-01-CreateYourWorldSunEtAl2023/images_for_zotero-1-x63-y447.png)





### 1 INTRODUCTION 



> [!Highlight]
> the underlying hypothesis is that the pre-trained diffusion model can be updated with a few samples of the new concept, which motivates we design a Lifelong text-to-image Diffusion Model (L2DM) via considering lifelong-task learning and multi-concept generation. (2 )
>  




### 2 RELATED WORK 




### 3 LIFELONG TEXT-TO-IMAGE DIFFUSION MODEL (L2DM) 




### 3.1 Revisit Text-to-Image Diffusion Model 




### 3.2 Problem Definition for Lifelong Text-to-Image Diffusion 




### 3.3 The Proposed L2DM Framework 




### 3.3.1 Lifelong-task Learning 



> [!Highlight]
> learned personalized concepts. (5 )
>  



> [!Highlight]
> Generate images {ˆ xiℓ}η i=1 for each piℓ in B1:k−1 l; (5 )
>  


> [!Highlight]
**Comment**:
> 각 prompt(task)마다 에타 개만큼 이미지를 생성 

> [!Highlight]
> for f ℓ l in B1:k−1 l do (5 )
>  


> [!Highlight]
**Comment**:
> 에타 * l 개의 이미지에 대해서 

> [!Highlight]
> The first term in Eq. (5) is designed to assess the catastrophic forgetting degree in generated images; (6 )
>  



> [!Highlight]
> spatial proximity of two concepts (6 )
>  



> [!Highlight]
> significant representation disparity of two concepts (6 )
>  



> [!Highlight]
> In other words, when two concepts are closely positioned or when the representation values of one concepts significantly outweighs that of the other, (6 )
>  



> [!Highlight]
> conceptslevel unmix loss and a dynamic attend loss (7 )
>  



> [!Highlight]
> spatial proximity and representation disparity (7 )
>  



> [!Highlight]
> The concepts-level unmix loss (7 )
>  



> [!Highlight]
> Mi represents a mask utilized in this loss function, which selectively assigns a value of zero to half of the cross-attention map while assigning a value of one to the remaining half. (7 )
>  



> [!Highlight]
> For the challenge posed by a significant representation disparity of two concepts (7 )
>  



> [!Highlight]
> attributeneglecting issue (7 )
>  



> [!Highlight]
> mixing of attention distributions (7 )
>  



> [!Highlight]
> between personalized and prior concept tokens (7 )
>  



> [!Highlight]
> we develop an orthogonal attention artist module to effectively disentangle the attention distributions of individual tokens. (7 )
>  



> [!Highlight]
> orthogonal attention artist mechanism strengthens the representation of the personalized token (7 )
>  



> [!Highlight]
> when it matches the concept token (i.e., i = j) (7 )
>  




### 4 EXPERIMENTS 




### 4.1 Datasets and Evaluation 




### ⇑1.8 




### 4.2 Implementation Details 




### 4.3 Comparison Evaluation 




### 4.3.1 Lifelong Single-concept Generation Results 




### 4.3.2 Lifelong Multi-concept Generation Results 




### 4.3.3 Ablation Study 




### 5 CONCLUSION 



> [!Highlight]
> concept customization of text-to-image diffusion (14 )
>  



> [!Highlight]
> Svdiff (14 )
>  


---

