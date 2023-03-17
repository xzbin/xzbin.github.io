---
title: torchFM 代码解读系列-FFM
date: 2023-02-23 15:45:47
tags: 
    - 代码解读
    - 推荐系统
    - FFM
categories: 
  - 工程
---


* 原始论文：[Field-aware Factorization Machines for CTR Prediction](https://www.csie.ntu.edu.tw/~cjlin/papers/ffm.pdf)
* 代码示例：[ffm.py](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/ffm.py)

# 理论解读
* 计算公式: $$y=\sigma{(\sum_{i=0}^n{w_ix_i}+{\sum_{i=1}^{n-1}\sum_{i+1}^n { \langle v_{i,f_j},v_{j,f_i} \rangle} {x_ix_j}})}$$
* 解说：<$$ v_{i,f_j},v_{j,f_i}$$>$$ 是x_i,x_j 对应的第j个field特征向量$$，
* <\*,\*>是向量内积。
<!-- * 解读: $$ {\langle v_{i,f_j},v_{j,f_i} \rangle 是x_i,x_j 对应的第j个field特征向量} $$， -->

# 具体实现
## 整体部分
* `FM` 是由2部分来实现的，线性部分和交叉项部分。而且其所有的输入均为类别性特征，不存在连续值特征。所以整体实现如下：

![整体部分](./ffm.png)

## 线性部分
* `线性部分`：该部分的实现与`FM`的线性部分相同，故不做赘述。

![线性部分](./fm_1.png)

## 交叉项部分
* `交叉项部分`：该部分是对交叉项的实现，由于是`Field`维度的向量交互，所以在实现上使用了多个`Embedding`层。求内积部分：由于不能简化，只能使用最`O(n^2)`的复杂度实现

![交叉项部分](./ffm_2.png)