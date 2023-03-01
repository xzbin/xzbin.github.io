---
title: torchFM 代码解读系列
date: 2023-02-23 15:45:47
tags: 
    - 推荐系统
    - 代码解读
categories: 
  - 代码解读
---

今天开一个新系列，`torchFM代码解读`，以加深对模型结构的理解。[代码仓库](https://github.com/forrestneo/pytorch-fm)，虽然仓库非常老了，但是依旧有借鉴意义。这个仓库只是半成品，很多地方不够完美，可以借鉴其实现思路，但是最好不要完全复用。


# [FM](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/fm.py)

原始论文: [Factorization Machines](https://www.csie.ntu.edu.tw/~b97053/paper/Rendle2010FM.pdf)

## 理论解读
* 计算公式: $y=\sigma{(\sum_{i=0}^n{w_ix_i}+{\sum_{i=1}^{n-1}\sum_{i+1}^n{\left \langle v_i,v_j \right \rangle}{x_ix_j}})}$
* 解读: $v_i,v_j 是x_i,x_j 对应的特征向量$，<\*,*>是向量内积
  
## 具体实现
### 整体部分
* `FM` 是由2部分来实现的，线性部分和交叉项部分。而且其所有的输入均为类别性特征，不存在连续值特征。所以整体实现如下：

![整体部分](./fm.png)

### 线性部分
* `线性部分`：该部分的实现主要依赖`Embedding` 映射，其中`offsets`部分可以忽略，该部分只对二维的数据起作用，而且用以映射embedding。

![线性部分](./fm_1.png)

### 交叉项部分
* `交叉项部分`：该部分是对交叉项的实现，而且使用了效率最高的实现方式。原公式中交叉项的时间复杂度是$O(kn^2)$，经过简化 时间复杂度可以变成$O(kn)$。这部分网上有大把资料不做赘述。

![交叉项部分](./fm_2.png)


# [FFM](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/ffm.py)

原始论文：[Field-aware Factorization Machines for CTR Prediction](https://www.csie.ntu.edu.tw/~cjlin/papers/ffm.pdf)

## 理论解读
* 计算公式: $y=\sigma{(\sum_{i=0}^n{w_ix_i}+{\sum_{i=1}^{n-1}\sum_{i+1}^n { \langle v_{i,f_j},v_{j,f_i} \rangle} {x_ix_j}})}$
* 解读: $ \langle v_{i,f_j},v_{j,f_i} \rangle  是x_i,x_j 对应的第j个field特征向量$，<\*,*>是向量内积

## 具体实现
### 整体部分
* `FM` 是由2部分来实现的，线性部分和交叉项部分。而且其所有的输入均为类别性特征，不存在连续值特征。所以整体实现如下：

![整体部分](./ffm.png)

### 线性部分
* `线性部分`：该部分的实现与`FM`的线性部分相同，故不做赘述。

![线性部分](./fm_1.png)

### 交叉项部分
* `交叉项部分`：该部分是对交叉项的实现，由于是`Field`维度的向量交互，所以在实现上使用了多个`Embedding`层。求内积部分：由于不能简化，只能使用最`O(n^2)`的复杂度实现

![交叉项部分](./ffm_2.png)

# [HOFM](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/hofm.py)

原始论文：[Higher-order factorization machines](https://arxiv.org/abs/1607.07195)

# [FNN](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/fnn.py)
原始论文：[Deep Learning over Multi-field Categorical Data: A Case Study on User Response Prediction](https://arxiv.org/abs/1601.02376)

# [Wide & Deep](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/wd.py)
原始论文: [Wide & Deep Learning for Recommender Systems](https://arxiv.org/abs/1606.07792)

## 理论解读
* 关于该模型结构，相对比较好理解。`wide`部分和 `Deep`部分。
![wide & deep](./wd.png)
## 具体实现
### 整体部分
* `Wide & Deep` 是由2部分来实现的，`wide`部分和 `Deep`部分。但是理论上其输入既包含了类别性特征，又存在连续值特征。但是该实现只考虑了类别性特征。整体实现如下：

![wide & deep](./wd2.png)

### Wide 部分
* `Wide 部分`：该部分的实现与`FM`的线性部分相同，故不做赘述。
### Deep 部分
* `Deep 部分`：结构简单，激活函数为`Relu`的`MLP`.

![Deep 部分](./wd2_2.png)

# [AFN](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/afn.py)
原始论文: [Attentional Factorization Machines: Learning the Weight of Feature Interactions via Attention Networks](https://arxiv.org/abs/1708.04617)

# [NFM](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/nfm.py)
原始论文: [Neural Factorization Machines for Sparse Predictive Analytics](https://arxiv.org/abs/1708.05027)


# [NCF](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/ncf.py)
原始论文: [Neural Collaborative Filtering](https://arxiv.org/abs/1708.05031)


# [FNFM](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/fnfm.py)
原始论文: [Field-aware Neural Factorization Machine for Click-Through Rate Prediction](https://arxiv.org/abs/1902.09096)


# [PNN](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/pnn.py)
原始论文: [Product-based Neural Networks for User Response Prediction](https://arxiv.org/abs/1611.00144)


# [DCN](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/dcn.py)
原始论文: [Deep & Cross Network for Ad Click Predictions](https://arxiv.org/abs/1708.05123)


# [DeepFM](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/dfm.py)
原始论文: [DeepFM: A Factorization-Machine based Neural Network for CTR Prediction](https://arxiv.org/abs/1703.04247)

# [xDeepFM](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/xdfm.py)
原始论文: [xDeepFM: Combining Explicit and Implicit Feature Interactions for Recommender Systems](https://arxiv.org/abs/1803.05170)


# [AFI](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/afi.py)
原始论文: [AutoInt: Automatic Feature Interaction Learning via Self-Attentive Neural Networks](https://arxiv.org/abs/1810.11921)

# [AFN](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/afn.py)
原始论文: [Adaptive Factorization Network: Learning Adaptive-Order Feature Interactions](https://arxiv.org/pdf/1909.03276.pdf)