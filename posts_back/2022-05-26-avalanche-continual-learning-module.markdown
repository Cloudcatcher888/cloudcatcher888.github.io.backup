---
layout: post
title:  "Avalanche: A Continual Learning Python Module"
date:   2022-05-26
categories: jekyll update
---
# 使用Avalanche快速实现增量学习的实验



[参考](https://avalanche-api.continualai.org/en/v0.1.0/index.html)

主要分为

- [Benchmarks module](https://avalanche-api.continualai.org/en/v0.1.0/benchmarks.html)
- [Evaluation module](https://avalanche-api.continualai.org/en/v0.1.0/evaluation.html)
- [Logging module](https://avalanche-api.continualai.org/en/v0.1.0/logging.html)
- [Models module](https://avalanche-api.continualai.org/en/v0.1.0/models.html)
- [Training module](https://avalanche-api.continualai.org/en/v0.1.0/training.html)

五个模块，其中benchmarks是各种增量数据集，以CV领域的为主。Models模块都是基础模型，因为增量学习基本是模型正交的，所以这里面不是重点。重点在于Training模块，里面包含了各种增量学习的策略，具体的常见流程如下：

```python
import torch
from torch.nn import CrossEntropyLoss
from torch.optim import SGD

from avalanche.benchmarks.classic import PermutedMNIST
from avalanche.models import SimpleMLP
from avalanche.training import EWC


# Config
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
# model
model = SimpleMLP(num_classes=10)

# CL Benchmark Creation
perm_mnist = PermutedMNIST(n_experiences=5) #任务数量，对permuted数据集而言就是采用了5中不同的像素随机打乱方式
train_stream = perm_mnist.train_stream
test_stream = perm_mnist.test_stream

# Prepare for training & testing
optimizer = SGD(model.parameters(), lr=0.001, momentum=0.9)
criterion = CrossEntropyLoss()

# Continual learning strategy
cl_strategy = EWC(
    model, optimizer, criterion, ewc_lambda=1, train_mb_size=32, train_epochs=2,
    eval_mb_size=32, device=device) #策略里面大部分参数是统一的，但也有各个策略专属的参数，例如EWC的ewc_lambda

# train and test loop over the stream of experiences
results = []
for train_exp in train_stream:#每次迭代运行一个task
    cl_strategy.train(train_exp) #task的训练
    results.append(cl_strategy.eval(test_stream)) #task的评价
```

