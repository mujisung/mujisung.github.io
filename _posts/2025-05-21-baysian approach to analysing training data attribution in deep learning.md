---
layout: posts
title: sumarize paper called 'A Bayesian Approach To Analysing Training Data Attribution In Deep Learning'
---

# motivation: sumarize paper 'A Bayesian Approach To Analysing Training Data Attribution In Deep Learning'



이 논문은 **“A Bayesian Approach to Analysing Training Data Attribution in Deep Learning”** (Elisa Nguyen et al., NeurIPS 2023)에 관한 것입니다. 간단히 요약하면, 이 논문은 **딥러닝 모델에서 특정 테스트 샘플의 예측에 영향을 미친 훈련 샘플을 찾아주는 Training Data Attribution (TDA)** 기법을 **베이지안 관점에서 재해석**하고, 이 기법들이 실제로 신뢰할 수 있는지를 분석한 연구입니다.

---

### 🧠 핵심 아이디어 요약:

#### 🔍 **1. Training Data Attribution (TDA)**란?

- 어떤 테스트 샘플 `z`의 예측에 대해, 훈련 데이터 `z_j`가 얼마나 영향을 주었는지를 수치화함.
    
- 기존에는 Leave-One-Out (LOO) 방식으로 `z_j`를 제거하고 다시 모델을 학습시켜서 테스트 샘플 `z`의 loss 변화량을 측정했음.
    
    τ(zj,z)=L(z;θ−j)−L(z;θ)\tau(z_j, z) = L(z; \theta_{-j}) - L(z; \theta)τ(zj​,z)=L(z;θ−j​)−L(z;θ)

#### ⚠️ **2. 기존 TDA의 한계점**

- 딥러닝에서는 모델이 **초기화(random init)**나 **SGD 배치 순서**에 따라 크게 달라짐 → TDA 결과도 민감하게 달라짐.
    
- Influence Function, Grad-Dot, Grad-Cos 같은 기존 TDA 방법은 이러한 변동성을 무시한 채 단일 모델로만 계산함 → 결과가 불안정함.
    

#### 📊 **3. 논문의 핵심 기여**

- **TDA 값을 확률 변수로 취급**하고, 여러 번 모델을 학습해 **평균과 분산(μ, σ)**을 추정.
    
- 이로 인해 "진짜 TDA가 noise에 의해 영향을 받는 정도"를 수치적으로 검증 가능.
    
- TDA 결과가 안정적인 경우 (low-noise pair)만 신뢰할 수 있으며, 그런 경우가 흔치 않다는 것을 실험적으로 보임.
    

#### 🔬 **4. 주요 실험 결과**

- 다양한 조건(CNN, ViT, 작은/큰 데이터셋, SWA/Ensemble 초기화 등)에서 TDA의 신뢰도(p-value)를 측정.
    
- 모델이 복잡할수록, 데이터셋이 클수록, 초기화가 달라질수록 TDA 결과는 더 **불안정**해짐.
    
- 특히 ViT 같은 대형 모델에서는 거의 모든 TDA 결과가 noise에 의해 흔들림 → 신뢰 어려움.
    

#### ✅ **5. 제안 사항**

- TDA를 사용할 때는 평균뿐 아니라 분산까지 고려해 **TDA score의 신뢰성**을 평가해야 함.
    
- 추후 연구자는 TDA 방법을 설계하거나 쓸 때 **베이지안 관점**으로 보고, **low-noise pair** 위주로만 사용하라고 권고.
    

---

### 🧾 결론 요약:

이 논문은 "딥러닝에서 TDA 기법은 대부분 noise에 취약하고 불안정하다"는 점을 강조하며, TDA를 **point estimate가 아닌 분포(distribution)**로 봐야 하며, 신뢰할 수 있는 평가를 위해 **베이지안 관점**을 도입해야 한다고 주장합니다.