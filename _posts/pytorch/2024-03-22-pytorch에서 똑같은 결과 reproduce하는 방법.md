---
layout: posts
title: pytorch에서 똑같은 결과 reproduce하는 방법
categories:
  - pytorch
tags:
  - pytorch
author_profile: false
sidebar:
  nav: counts
search: true
---

<div class="notice--info" markdown="1" style='font-size: 20px'>
**motivation:**  can't reproduce results even set all random seeds in pytorch.
</div>

# 질문: can't reproduce results even set all random seeds in pytorch.

</br>
</br>
</br>


# 답변: 

Not being able to reproduce results in PyTorch, even after setting all random seeds, can be frustrating. It's crucial to ensure that every possible source of randomness is controlled. Here are some steps and aspects to consider to ensure reproducibility in your experiments:

1. **Set Random Seeds Everywhere:** Make sure you've set the random seed for PyTorch, NumPy, and the Python random module. Additionally, if you're using other libraries (like random number generators in CUDA), you'll need to set their seeds as well.
    
    pythonCopy code
    
    `import torch import numpy as np import random  torch.manual_seed(0) np.random.seed(0) random.seed(0) if torch.cuda.is_available():     torch.cuda.manual_seed_all(0)`
    
2. **Control CUDA Non-Determinism:** Some operations on CUDA are not deterministic by default. You might need to explicitly ask PyTorch to use deterministic algorithms. This can impact performance but ensures reproducibility.
    
    pythonCopy code
    
    `torch.backends.cudnn.deterministic = True torch.backends.cudnn.benchmark = False`
    
3. **Understand Data Loader Workers:** If you're using the `DataLoader` with multiple workers (`num_workers > 0`), the way data is fetched might introduce randomness. Ensure the data loading process is deterministic by setting the `worker_init_fn` in the DataLoader.
    
    pythonCopy code
    
    `def seed_worker(worker_id):     worker_seed = torch.initial_seed() % 2**32     np.random.seed(worker_seed)     random.seed(worker_seed)  loader = torch.utils.data.DataLoader(dataset, worker_init_fn=seed_worker)`
    
4. **Model Initialization:** Ensure that the model weights are initialized deterministically. If you're using a custom initialization method, make sure it's using a fixed seed or a deterministic algorithm.
    
5. **Environment:** Even with all the seeds set correctly, running the code on different hardware or software environments (different versions of CUDA, PyTorch, or even different GPUs) can lead to slight variations in results.
    
6. **Version Control:** Ensure that you're using the same versions of PyTorch, CUDA, and any other dependencies, as updates can change the underlying implementations of algorithms and operations.
    
7. **Review Any External Sources of Randomness:** If your code depends on external systems or data sources, make sure they are also providing data deterministically.
    
8. **Floating Point Arithmetic:** Be aware that floating-point arithmetic can introduce non-determinism, especially with operations that have non-associative properties (like addition and multiplication). This is more pronounced when using GPUs.
    

If after all these steps you still face issues with reproducibility, consider isolating the part of the code that leads to different results across runs. It can sometimes be a specific operation or layer in your model causing the issue.

