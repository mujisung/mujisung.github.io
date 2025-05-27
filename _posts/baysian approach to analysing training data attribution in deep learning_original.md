---
layout: posts
title: sumarize paper called 'A Bayesian Approach To Analysing Training Data Attribution In Deep Learning'
---

# motivation: sumarize paper 'A Bayesian Approach To Analysing Training Data Attribution In Deep Learning'



## **Blog Post: Rethinking TDA – A Bayesian Approach to Understanding Data Influence in Deep Learning**

**Introduction**

Training Data Attribution (TDA) aims to explain model predictions by identifying which training samples most influenced a specific decision. This data-centric explanation method has gained popularity, especially as models become more opaque and widely deployed. Conventionally, methods like Influence Functions estimate the effect of removing a training point on the loss for a test sample. These methods, however, are typically built on strong assumptions—like model convexity—and ignore the stochastic nature of deep learning.

In “A Bayesian Approach to Analysing Training Data Attribution in Deep Learning,” the authors challenge this assumption and propose a novel, Bayesian lens for TDA. They argue that TDA values should be treated not as deterministic outputs, but as random variables subject to the inherent noise of deep neural network (DNN) training. This work introduces a probabilistic reformulation of TDA, bringing new rigor and caution to a growing field.

---

## **Background**

TDA methods trace how the removal of a specific training sample $$z_j$$ impacts a test prediction $$z$$ by comparing the loss of the full model fθf_\thetafθ​ with a retrained model fθ∖jf_{\theta_{\setminus j}}fθ∖j​​ that excludes zjz_jzj​. While this leave-one-out (LOO) procedure provides a “ground-truth” influence measure, it is computationally infeasible for modern DNNs. Therefore, various approximation methods have emerged:

- **Influence Functions** (IF): Estimate influence using gradients and the inverse Hessian. Fragile in deep networks due to non-convexity and high variance.
    
- **Gradient-Based Methods**: Such as Grad-Dot and Grad-Cos, use similarity in gradient directions to estimate influence without requiring second-order computations.
    
- **Additional Training Step (ATS)**: Measures the effect of retraining _with_ an extra sample instead of without.
    

Despite their efficiency, these methods suffer from instability, often yielding contradictory results depending on random seeds and training setups.

---

## **A Bayesian Perspective on TDA**

The core insight of this paper is that **model training in deep learning is inherently stochastic**. As such, any TDA estimate τ(zj,z)\tau(z_j, z)τ(zj​,z) must be viewed as a distribution rather than a single number.

### Why?

Because the model parameters θ\thetaθ are sampled from a posterior distribution p(θ∣D)p(\theta|D)p(θ∣D), due to:

- Random initialization
    
- Mini-batch sampling in SGD
    

Hence, both the full model θ\thetaθ and the leave-one-out model θ∖j\theta_{\setminus j}θ∖j​ are random variables. This reframing naturally leads to sampling-based estimation of TDA, capturing both **mean** and **variance** of influence scores.

The authors propose two methods to sample from this posterior:

- **Deep Ensembles (DE)**: Training multiple models from different initializations or batch orderings.
    
- **Stochastic Weight Averaging (SWA)**: Sampling from SGD checkpoints during training.
    

---

## **Experimental Setup**

The experiments aim to quantify TDA uncertainty and understand when attribution is reliable.

### Datasets:

- **MNIST3** (custom 3-class subset)
    
- **CIFAR-10** (subset for feasibility)
    

### Models:

- **CNN-2L / CNN-3L**: Shallow CNN baselines
    
- **ViT + LoRA**: Lightweight transformer-based model
    

### Metrics:

- **TDA Estimations**: IF, Grad-Dot, Grad-Cos, ATS
    
- **Ground Truth**: True LOO
    
- **Evaluation**: Mean, variance, and statistical significance of influence scores using **Student’s t-test**
    

---

## **Key Findings**

### 1. **TDA Estimates Are Unreliable**

Even “ground-truth” LOO influence is highly variable across different training runs. For large datasets or complex models, variance in TDA values increases significantly. This undermines the reliability of all TDA methods when treated deterministically.

### 2. **Influence of Randomness Sources**

- **Initialization** affects global model behavior and has a large impact on TDA variance.
    
- **Batch Order** during SGD affects local behavior, producing relatively stable TDA values.
    

This insight aligns with Bayesian intuition: different global optima correspond to different functionally distinct models.

### 3. **Model Complexity and Dataset Size**

- Larger datasets initially increase TDA variance due to more batch combinations.
    
- Eventually, variance decreases since individual data points exert less influence.
    
- More complex models yield noisier TDA scores due to increased instability.
    

### 4. **Statistical Significance Matters**

Rather than using raw TDA scores, the authors argue for assessing **statistical significance**. Many scores with high variance are misleadingly interpreted as meaningful.

---

## **Conclusion**

This work offers a much-needed reality check for TDA research. It proposes that attribution is inherently uncertain and must be analyzed through a probabilistic lens. Key contributions include:

1. **Bayesian formulation of TDA**, making influence a distribution—not a number.
    
2. **Empirical analysis** revealing high variance and limited reliability in existing methods.
    
3. **Practical guidelines** to compute and evaluate influence more responsibly.
    

The takeaway is clear: **use TDA only when it is statistically justified**—when variance is low. Practitioners should avoid over-reliance on deterministic influence values in high-stakes settings.

---

## **Strengths**

✅ **Theoretical Depth**: A rigorous probabilistic rethinking of a widely-used tool.  
✅ **Clear Practical Recommendations**: Emphasizes when and how to trust TDA.  
✅ **Strong Empirical Backing**: Uses both CNN and transformer models across datasets.

## **Limitations**

⚠️ **Limited Dataset Complexity**: Only tested on small-scale data like MNIST and CIFAR-10.  
⚠️ **No Real-World Benchmarks**: Needs extension to NLP or vision tasks with societal stakes.  
⚠️ **Gradient-Based Only**: Non-gradient TDA methods (e.g., prototype-based) are not evaluated.

---

## **References**

- Koh & Liang. "Understanding Black-box Predictions via Influence Functions." ICML 2017.
    
- Charpiat et al. "Input Similarity from the Neural Network Perspective." NeurIPS 2019.
    
- Karthikeyan & Søgaard. "Revisiting Methods for Finding Influential Examples." arXiv.
    

---

Let me know if you'd like a version formatted for Medium, Markdown, or a slide deck summary version too!