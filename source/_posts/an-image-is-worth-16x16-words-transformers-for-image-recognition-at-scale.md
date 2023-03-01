---
title: 'an image is worth 16x16 words: transformers for image recognition at scale'
date: 2023-02-24 19:39:08
tags:
    - 图像
    - vit
    - backbone
categories:
  - [论文阅读]
---

# 文章主旨
* 随着`transformer`在NLP领域取得巨大的成功，其在图像处理领域的应用也有很多尝试，本文是该方向上取得了突破，而且取得了不错的结果，从这个角度来讲，意义非凡。
* 如果说是`transorfer`结构，其实不太准确，准确来讲应该是使用了`transformer-Encoder`。

# 模型结构

![vit](./vit.png)

* 在`CNN`模型中，图像可以直接输入模型。但是在`transformer`结构不同，需要进行`patch`处理：即将图像划分成等大小的方块。
* 具体处理如下：图像大小：`384*384`，path: `16*16`，那么最终得到的`token`数量：`(384/16) * (384/16) + 1 = 577`。此处加`1`是因为 `[CLS]` token。
* 此处图像的尺寸变化: 
  * $X\in R^{H\*W\*C}$ 
  * $X_P\in R^{(N+1)\* (P\*P\*C)}$ 
  * $X_P\in R^{(N+1)\*D}$

## 用公式表示
![vit formula](./vit_1.png)

* 从$z_0$ 表示可以看出 $E$ 是用来调整输入的维度 $(P\*P\*C) \rightarrow D $

# 模型实验
## 数据集
  * `ILSVRC-2012 ImageNet`:  1k classes and 1.3M images
  * `ImageNet-21k`: 21k classes and 14M images
  * `JFT`: 18k classes and 303M high-resolution images.
  * `CIFAR-10/100`
  * `Oxford-IIIT Pets`
  * `Oxford Flowers-102`
## 实验结果
与 `BiT-L` 与 `Noisy Student`对比，效果均有提升
![vit](./vit_2.png)