---
title: >-
  LoGAN: Generating Logos with a Generative Adversarial Neural Network
  Conditioned on color
categories:
  - [论文阅读]
  - [图像]
date: 2021-06-16 13:43:44
tags:
  - GAN
---


本文提出了`LoGAN`框架用于logo生成，基于`AC-WGAN-GP`结构。其`condition` 是12种不同的颜色，以颜色进行logo类别的标识。该篇论文代码已经开源，[下载地址](https://github.com/ajki/LoGAN)

总体上说来，论文思路很简单，但是有很强的业界价值。



`LogoGAN`框架图如下图所示：

![logoGAN](./logoGAN.png)



* 该模型综合了`AC-GAN`与 `WGAN-GP` 两部分的优势，又进行了部分改造：
  * `AC-GAN` 的`D` 损失来源于两个部分：`real-fake loss` 和 `class loss`。
  * `WGAN-GP` 稳定了训练过程。
  * `LoGAN`的 `D` 的损失只来自于`real-fake loss`。由增加的分类器 `Q ` 进行  `class loss`的判定。不过从图中结构来看`Q`的`feature map` 也应该来自于`D`。



* 数据使用的是 ` LLD-icons dataset`, 有 `486’377 32 *×* 32 icon`。
* 使用`k-means`算法找到每张图片上最主要的3种颜色，最主要的是组成`color words`。全部一共有12种主要的颜色：`black, blue, brown, cyan, gray, green, orange, pink, purple,red, white, and yellow`

* 模型评估使用的是`Precision `、`Recall `、`F1`。

实际效果展示：

![logo](./logo.png)

综合评价：

以目前的方案来讲实用性不是很大，更多的是给予一种可能性。距离实际使用差距太大。

* 目前只考虑了logo的颜色，缺少太多定制化的要求，比如：特定图形、符号的融入。
* 分辨率过低，图像太过于模糊。
