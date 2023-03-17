---
title: torchFM 代码解读系列-wide & deep
date: 2023-03-03 09:49:34
tags: 
    - 代码解读
    - 推荐系统
    - Wide & Deep
categories: 
  - 工程
---

* 原始论文: [Wide & Deep Learning for Recommender Systems](https://arxiv.org/abs/1606.07792)
* 代码示例：[wd.py](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/wd.py)

# 理论解读
* 关于该模型结构，相对比较好理解。`wide`部分和 `Deep`部分。
![wide & deep](./wd.png)
# 具体实现
## 整体部分
* `Wide & Deep` 是由2部分来实现的，`wide`部分和 `Deep`部分。但是理论上其输入既包含了类别性特征，又存在连续值特征。但是该实现只考虑了类别性特征。整体实现如下：

![wide & deep](./wd2.png)

## Wide 部分
* `Wide 部分`：该部分的实现与`FM`的线性部分相同，故不做赘述。

## Deep 部分
* `Deep 部分`：结构简单，激活函数为`Relu`的`MLP`.

![Deep 部分](./wd2_2.png)