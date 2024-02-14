---
layout: posts
title: Proxy Anchor-based Unsupervised Learning for Continuous Generalized Category Discovery
categories: [paper review, AI]
tag: [Proxy Anchor, Unsupervised, CGCD  ]  
author_profile: false # 글을 누르면 내 소개가 없어짐. true로 하면 얼굴이 나옴.
sidebar:      # 글을 누르면 목차가 나온다.
  nav: "counts" 
search: true     # false라고 하면 글이 검색이 안된다.
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:**  understanding CGCD
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
> [1]  H. Kim, S. Suh, D. Kim, D. Jeong, H. Cho와/과J. Kim, “Proxy Anchor-based Unsupervised Learning for Continuous Generalized Category Discovery”. arXiv, 2023년 11월 2일. 접근된: 2024년 2월 13일. [Online]. Available at: [http://arxiv.org/abs/2307.10943](http://arxiv.org/abs/2307.10943)

> [!Synth]
> **Contribution::**

> [!Md]
>
> **Author**:: Kim, Hyungmin> 
>
> **Author**:: Suh, Sungho> 
>
> **Author**:: Kim, Daehwan> 
>
> **Author**:: Jeong, Daun> 
>
> **Author**:: Cho, Hansang> 
>
> **Author**:: Kim, Junmo> 
>
> **Title**:: Proxy Anchor-based Unsupervised Learning for Continuous Generalized Category Discovery
> **Year**:: 2023
> **Citekey**:: [[@2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023]]
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
> [Kim 등 - 2023 - Proxy Anchor-based Unsupervised Learning for Conti.pdf](C:\Users\wys00\Zotero\storage\UQBKBGG3\Kim%20등%20-%202023%20-%20Proxy%20Anchor-based%20Unsupervised%20Learning%20for%20Conti.pdf)

> [!Abstract]
>
> abstract:: Recent advances in deep learning have significantly improved the performance of various computer vision applications. However, discovering novel categories in an incremental learning scenario remains a challenging problem due to the lack of prior knowledge about the number and nature of new categories. Existing methods for novel category discovery are limited by their reliance on labeled datasets and prior knowledge about the number of novel categories and the proportion of novel samples in the batch. To address the limitations and more accurately reflect real-world scenarios, in this paper, we propose a novel unsupervised class incremental learning approach for discovering novel categories on unlabeled sets without prior knowledge. The proposed method fine-tunes the feature extractor and proxy anchors on labeled sets, then splits samples into old and novel categories and clusters on the unlabeled dataset. Furthermore, the proxy anchors-based exemplar generates representative category vectors to mitigate catastrophic forgetting. Experimental results demonstrate that our proposed approach outperforms the state-of-the-art methods on finegrained datasets under real-world scenarios. 
>

---

# Annotations

### 1. Introduction 


 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-2-x42-y589.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-2-x44-y447.png)





### 2. Problem Definition 




### 2.1. Continuous Generalized Category Discovery 


 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-3-x44-y465.png)





### 2.2. Setting of Continuous Generalized Category Discovery 


 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-3-x303-y311.png)





### 3. Method 




### 3.1. Initial Step: Fine Tune 


 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-4-x47-y135.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-4-x298-y614.png)





### 3.2. Discovering Novel Categories Step 




### Separation: 


 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-4-x336-y365.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-4-x309-y226.png)





### Pseudo-labeling: 




### 3.3. Category Incremental Step 




### Training modified model and PAs: 


 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-5-x52-y468.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-5-x61-y295.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-5-x68-y204.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-5-x88-y70.png)





### 4. Experimental Results 




### 4.1. Implementation Details 




### 4.2. Evaluation Metrics 


 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-5-x357-y337.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-5-x370-y180.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-6-x44-y601.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-6-x44-y352.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-7-x44-y603.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-7-x304-y554.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-7-x302-y446.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-8-x45-y594.png)





### 5. Related work 




### 5.1. Novel Category Discovery 




### 5.2. Image Retrieval 




### 5.3. Noise Label 




### 6. Conclusion 




### Acknowledgements 


 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-12-x38-y588.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-12-x43-y307.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-14-x44-y119.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-15-x45-y119.png)



 


![](images_for_zotero/2023-01-01-ProxyAnchorbasedUnsupervisedKimEtAl2023/images_for_zotero-16-x46-y124.png)



---

