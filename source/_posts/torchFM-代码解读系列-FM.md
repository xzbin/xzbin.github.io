---
title: Factorization Machines
date: 2023-02-23 15:45:47
tags: 
    - 代码解读
    - 推荐系统
    - FM
categories: 
  - 工程
---

* 原始论文: [Factorization Machines](https://www.csie.ntu.edu.tw/~b97053/paper/Rendle2010FM.pdf)
* 代码实例：[fm.py](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/fm.py)

# 理论解读
* 计算公式: $$y=\sigma{(\sum_{i=0}^n{w_ix_i}+{\sum_{i=1}^{n-1}\sum_{i+1}^n{\left \langle v_i,v_j \right \rangle}{x_ix_j}})}$$
* 解读: $v_i,v_j 是x_i,x_j 对应的特征向量$，<\*,*>是向量内积
  
# 具体实现
## 整体部分
* `FM` 是由2部分来实现的，线性部分和交叉项部分。而且其所有的输入均为类别性特征，不存在连续值特征。所以整体实现如下：

![整体部分](./fm.png)

## 线性部分
* `线性部分`：该部分的实现主要依赖`Embedding` 映射，其中`offsets`部分可以忽略，该部分只对二维的数据起作用，而且用以映射embedding。

![线性部分](./fm_1.png)

## 交叉项部分
* `交叉项部分`：该部分是对交叉项的实现，而且使用了效率最高的实现方式。原公式中交叉项的时间复杂度是$O(kn^2)$，经过简化 时间复杂度可以变成$O(kn)$。这部分网上有大把资料不做赘述。

![交叉项部分](./fm_2.png)