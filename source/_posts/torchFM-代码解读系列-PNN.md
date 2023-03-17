---
title: torchFM 代码解读系列-PNN
date: 2023-02-23 15:45:47
tags: 
    - 代码解读
    - 推荐系统
    - PNN
categories: 
  - 工程
---

* 原始论文: [Product-based Neural Networks for User Response Prediction](https://arxiv.org/abs/1611.00144)
* 代码示例：[pnn.py](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/pnn.py)

$$s_0 = node_0$$
$$s_1 = s_0 + randomNByCost(neighbor(s_0)) $$
$$s_2 = s_1 + randomNByCost(neighbor(s_1)) $$
$$.....$$
$$s_n = s_{n-1} + randomNByCost(neighbor(s_{n-1})) $$
$$s_n = s_{0} + \sum_{i=1}^nrandomNByCost(neighbor(s_{i-1})) $$

<!-- $$s_n = s_0 + \sum_{step=2}^{n}CostWeight *neighbors(s_{i-1})$$ -->

