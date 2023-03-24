---
title: 'xDeepFM: Combining Explicit and Implicit Feature Interactions for Recommender Systems'
date: 2023-02-23 15:45:47
tags: 
    - xDeepFM
categories: 
  - [代码解读]
  - [推荐系统]
---


* 原始论文: [xDeepFM: Combining Explicit and Implicit Feature Interactions for Recommender Systems](https://arxiv.org/abs/1803.05170)
* 代码示例[xdfm.py](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/xdfm.py)

# 文章idea
* 交叉特征对`CTR`的预测至关重要，但是手动特征工程有很多弊端。
* `FM`：可以对所有的特征进行交叉，但是这样会引入无效特征，影响性能 ;`FNN`与`PNN`只侧重了高阶特征交叉，忽略了低阶。`Wide & Deep` 和 `DeepFM`通过引入`hybrid architectures`的方式联合了高阶和低阶特征。
* `DNN`以隐式的方式交叉特征，而且是`bit-wise`;`FM`是`vector-wise`的方式。
* `DCN`：特征交叉方式不够完美。引入了一种特殊类型的交叉特征。
* 因此作者提出了显式的、`vector-wise`方式的模型，并且可以随着网络深度的递增，增加交叉特征的`degree`。

# 文章主旨
* 提出了 `xDeepFM`，结合了显式与隐式的特征交叉，而且不需要手动特征工程。
* 提出了`compressed interaction network (CIN)` 用以学习显式的特征交叉。
# 模型细节
![xdfm.png](./xdfm.png)

# 模型实验
# 具体实现