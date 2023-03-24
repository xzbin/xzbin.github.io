---
title: 'DeepFM: A Factorization-Machine based Neural Network for CTR Prediction'
date: 2023-02-23 15:45:47
tags: 
    - DeepFM
categories: 
  - [代码解读]
  - [推荐系统]
---

* 原始论文: [DeepFM: A Factorization-Machine based Neural Network for CTR Prediction](https://arxiv.org/abs/1703.04247)
* 代码示例：[dfm.py](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/dfm.py)


# 文章idea
* 文章认为`high-order`交叉特征对`CTR`预估非常有效。但是`LR`需要大量的特征工程工作；`CNN`更加倾向于相邻特征的交互；`RNN`更适合处理序列化数据；`FNN`与`PNN`只捕捉了`low-order`交叉特征；`Wide & Deep` 也同样需要大量特征工程。
* 为了省去特征工程的麻烦，拟合`low-order`与`high-order`的特征，因此作者提出了`DeepFM`.

# 文章主旨
* 作者将`DNN`与`FM`结合，提出了`DeepFM`。与`Wide & Deep` 不同，其不需要特征工程。
* 与`Wide & Deep`不同，`DeepFM` 的`DNN`与`FM`两部分共享了输入。
# 模型细节

$$Y=sigmoid(Y_{FM} + Y_{DNN})$$

## FM Component
![dfm_fm.png](./dfm_fm.png)
这部分是标准的`fm`，具体细节不做赘述。公式如下：
$$y=\sigma{(\sum_{i=0}^n{w_ix_i}+{\sum_{i=1}^{n-1}\sum_{i+1}^n{\left \langle v_i,v_j \right \rangle}{x_ix_j}})}$$
## DNN Component
![dfm_dnn.png](./dfm_dnn.png)
这部分就是典型的`DNN`模型，具体细节不做赘述。

## embedding layer
![dfm_embedding.png](./dfm_embedding.png)

* 每个`field`的长度也许不同，但是经过`embedding layer`映射得到的矩阵尺寸是相同的`k`，由上图可知，使用了`sum`操作。
  
# 模型试验
* 数据集:`Company∗`, `Criteo`
![dfm_exp.png](./dfm_exp.png)

# 具体实现
整体结构![dfm_fm2.png](./dfm_fm2.png)
* 后面的`DNN`部分就是常规的`MLP`，所以不需要过多赘述。
* 图中红色方框标注出的部分即是`FM`。该部分细节不需要过多赘述，详见[Factorization Machines](/2023/02/23/torchFM-代码解读系列-FM/) 一文。