---
title: >-
  ZERO and R2D2: A Large-scale Chinese Cross-modal Benchmark and a
  Vision-Language Framework
date: 2023-03-01 09:09:40
tags:
    - 多模态
    - R2D2
categories:
  - [论文阅读]
---
我个人对该文章提出的模型架构并不是很推崇，因为其设计不够优美，有点过于复杂了。
***

# 文章idea
* 中文多模态没有相应的数据集



# 文章主旨
该文章提出了中文的多模态模型 `R2D2`，并且发布了中文的多模态数据集 `ZERO`。

# 模型结构

## 数据库`Zero`介绍
* 预训练数据库 `Zero-Corpus`，是由2.5亿 图像和 7.5亿文本组成，约50亿图文对数据。
* 下游任务数据集，`Image-Caption Matching Dataset`：40w 图文对［图像，文本描述］。
* 下游任务数据集，`Image-Query Matching Dataset`：40w［图像，QUERY］
* 下游任务数据集，`Image-Caption Retrieval Dataset` 20w［ 以文搜图，以图搜文］
* 下游数据集，`Image-Query Retrieval Dataset` 20w［以文搜图，以图搜文］
* 下游数据集，`Flickr-30k-CNA Dataset` 针对 `Flickr-30k` 进行了高质量翻译和检查，数据集质量高于 `Flickr-30k-CN`

## 模型细节
![r2d2.png](./r2d2.png)
### Text Encoder
使用的是 `RoBerta-wwm-ext`，其他的不做赘述。
### Image Encoder
`Image Encoder` 使用的是 `VIT`，其他的不做赘述。
### Cross Encoder
图像与文本的特征交叉有两个塔，使用的均是基于`transformer-encoder`，只是稍微做了一些调整。上图可以看到细节。
### 损失函数 
损失函数有 3 个:`MLM`，`GCPR`，`FGR`
* MLM：不用过多赘述
* GCPR：本质上是对比损失，其考虑了多个 GPU 的反向传播情况？
* FGR：本质上是 `ITM`，但是根据模型的特殊结构，其用两部分组成的最终实现。

## Two-way Distillation
这部分是使用自监督的思想实现，使用了 `teacher-student` 范式。
## Target-guided Distillation

* 考虑到数据集中的噪音，因此并没有直接使用标签`y`，而是`soft target`
![r2d2_2.png](./r2d2_2.png)
* 此处的`soft target` 比较有意思。主要是2点：
1. 在原始目标`y`的基础上增加了一个可变项。$I_m$是送入`teacher`网络的图像，
2. 引入 `queue` 设计，保留最近几个`step`训练得到的中间变量，`text 表征`：$T_q$，`图像表征`：$I_q$。因此，此时计算loss的计算不仅仅局限于`batch`内部，考虑到模型训练而导致的队列中数据表征的有效性，每次迭代都将队列中的元素衰减权重设定为0.99。
* 以上设计可能考虑到`对比损失`需要大batch 才有好的效果，这也只是猜测。

## Feature-guided Distillation
* 使用了`mask`机制，送入`teacher`网络的是原始文本，送入`student`网络的是`mask` 之后的文本。
* 使用了自监督的思想，让`student`网络的输出拟合`teacher`网络
# 模型实验
![r2d2_1.png](./r2d2_1.png)