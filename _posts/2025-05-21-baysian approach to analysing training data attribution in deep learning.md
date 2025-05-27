---
layout: posts
title: sumarize paper called 'A Bayesian Approach To Analysing Training Data Attribution In Deep Learning'
---

# motivation: sumarize paper 'A Bayesian Approach To Analysing Training Data Attribution In Deep Learning'

# Blog Post: Rethinking TDA – A Bayesian Approach to Understanding Data Influence in Deep Learning

## Introduction

Training Data Attribution (TDA) aims to explain model behavior by identifying which training examples are most responsible for a particular prediction. Conventionally, this is treated deterministically: for a training sample $$z_j$$ and a test sample $$z$$, TDA is defined as the change in loss on $$z$$ when the model is retrained without $$z_j$$.

However, the paper *“A Bayesian Approach to Analysing Training Data Attribution in Deep Learning”* argues that this view is flawed for deep models. The authors propose a **Bayesian reformulation** of TDA, treating attribution as a distribution due to the inherent randomness in training (e.g., initialization and SGD). This perspective leads to a deeper understanding of when TDA estimates are meaningful—and when they’re not.

## Background

The classic definition of TDA is:

$$
\tau(z_j, z) := \mathcal{L}(f_{\theta_{\setminus j}}, z) - \mathcal{L}(f_\theta, z)
$$

- $$f_\theta$$: model trained on full dataset $$D$$  
- $$f_{\theta_{\setminus j}}$$: model trained on $$D \setminus \{z_j\}$$  
- $$\mathcal{L}$$: loss function

Computing this exactly is expensive because it requires retraining the model for every training sample. Approximation methods like **Influence Functions (IF)** or **Gradient-Dot/Grad-Cos** are often used instead.

Yet, these approximations assume a deterministic mapping from dataset to model parameters, which doesn’t hold in deep learning. The same dataset $$D$$ can yield different models due to:

- Random initialization  
- Batch order in SGD

Thus, the trained model parameters $$\theta$$ are actually drawn from a **posterior distribution** $$p(\theta | D)$$. Consequently, the TDA estimate $$\tau(z_j, z)$$ is itself a **random variable**, not a fixed value.

## A Bayesian View of TDA

In this Bayesian formulation:

- Both $$\theta$$ and $$\theta_{\setminus j}$$ are samples from distributions:  
  $$p(\theta | D)$$ and $$p(\theta_{\setminus j} | D \setminus \{z_j\})$$
- So the TDA value becomes:  
  $$
  \tau(z_j, z) \sim \mathcal{L}(f_{\theta_{\setminus j}}, z) - \mathcal{L}(f_\theta, z)
  $$

### How to sample from the posterior?

Two practical approaches:

1. **Deep Ensembles (DE)**: Train multiple models with different initializations or batch orders.
2. **Stochastic Weight Averaging (SWA)**: Use checkpoints during a single SGD run to approximate posterior samples.

## Experiments

### Datasets
- MNIST3 (150 train × 900 test = 135,000 pairs)
- CIFAR-10 (500 train × 500 test = 250,000 pairs)

### Models
- CNN2-L, CNN3-L (shallow networks)
- ViT + LoRA (Transformer with low-rank adapters)

### Evaluated TDA Methods
- Influence Function (IF)
- Grad-Dot, Grad-Cos
- Additional Training Step (ATS)
- Ground-Truth: Leave-One-Out (LOO)

### Posterior Sampling Methods
- DE-init: different initialization
- DE-batch: same init, different batch order
- SWA: use final $$T$$ checkpoints

### Statistical Analysis
- Estimate **mean** and **variance** of $$\tau(z_j, z)$$
- Use **Student’s t-test** to check statistical significance:  
  $$
  p\text{-value} < 0.05 \Rightarrow \text{signal dominates noise}
  $$  
  $$
  p\text{-value} > 0.05 \Rightarrow \text{noisy estimate, not significant}
  $$

## Findings

### 1. TDA Is Often Unreliable
Even the “ground-truth” LOO values are highly variable. IF and Grad-Dot behave similarly. Grad-Cos is somewhat stable on MNIST but fails on CIFAR-10.

### 2. Sources of Variability
- **Initialization** introduces more variance than batch order.
- **Model complexity** increases instability.
- **Dataset size** has a non-monotonic effect: initially more variance, but eventually stabilizes.

### 3. Agreement Between Methods
Expectations from various methods diverge substantially. The authors identify two rough clusters:
- Group 1: ATS, IF
- Group 2: Grad-Dot, Grad-Cos

Yet none aligns well with LOO ground-truth when viewed as a distribution.

## Conclusion

This work delivers a strong message: **TDA should be seen as a distribution**, not a point estimate.

**Key takeaways:**
1. TDA values depend on randomness in training.
2. Evaluate both **mean** and **variance** of TDA.
3. Only trust TDA when the variance is low and the result is statistically significant.

## Strengths
First to apply Bayesian analysis to TDA  
Clear criteria for when TDA is reliable  
Practical evaluation across multiple models and datasets

## Limitations
Focused on small-scale datasets (MNIST, CIFAR-10)  
Only gradient-based TDA methods evaluated  
No hyperparameter optimization in experiments

## References
-Koh & Liang (2017). *Influence Functions*, ICML.  
- Charpiat et al. (2019). *Gradient Similarity Methods*, NeurIPS.  
- Karthikeyan & Søgaard (2021). *Expected TDA*, arXiv.
