---
layout: post
title:  "Math Tools for Research"
date:   2022-01-29 16:58:04 +0800
categories: jekyll update
---
## Introduction

Here is a list of several useful math tools for research.

## Dirichlet distribution

sample a categorial distribution over K categories.

```python
import numpy as np
from scipy.stats import dirichlet
np.set_printoptions(precision=2)

def stats(scale_factor, G0=[.2, .2, .6], N=10000):
    samples = dirichlet(alpha = scale_factor * np.array(G0)).rvs(N)
    print("                          alpha:", scale_factor)
    print("              element-wise mean:", samples.mean(axis=0))
    print("element-wise standard deviation:", samples.std(axis=0))
    print()
    
for scale in [0.1, 1, 10, 100, 1000]:
    stats(scale)
                          alpha: 0.1
              element-wise mean: [ 0.2  0.2  0.6]
element-wise standard deviation: [ 0.38  0.38  0.47]

                          alpha: 1
              element-wise mean: [ 0.2  0.2  0.6]
element-wise standard deviation: [ 0.28  0.28  0.35]

                          alpha: 10
              element-wise mean: [ 0.2  0.2  0.6]
element-wise standard deviation: [ 0.12  0.12  0.15]

                          alpha: 100
              element-wise mean: [ 0.2  0.2  0.6]
element-wise standard deviation: [ 0.04  0.04  0.05]

                          alpha: 1000
              element-wise mean: [ 0.2  0.2  0.6]
element-wise standard deviation: [ 0.01  0.01  0.02]
```



