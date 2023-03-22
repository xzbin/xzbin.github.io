---
title: 'VLMO: Unified Vision-Language Pre-Training with Mixture-of-Modality-Experts'
date: 2023-02-28 20:53:33
tags:
    - VLMO
categories:
  - [论文阅读]
  - [多模态]
---

* 原始论文: [VLMO: Unified Vision-Language Pre-Training with Mixture-of-Modality-Experts](https://arxiv.org/abs/2111.02358)

# 文章idea
* 多模态架构中，`dual-encoder` 对图文检索任务非常友好，因为其可以分别`encode` 图文特征; `fusion-encoder` 对图文分类任务更加友好，因为其可以对图文特征进行深入融合。
* 为了更好的利用这两种架构的优势，作者提出了新的`transformer`模块：`MoME`；以及基于这种模块的新模型：`VLMo`；



# 文章主旨
* 提出了新的 `transformer` 结构 `MoME`，其中的 `self-attention` 结构可以对齐不同的模态信息，模态 `expert` 可以获取特定的模态信息。
* 提出了基于`MoME`结构的多模态模型 `VLMo`。也提出了预训练分阶段的预训练方案，提高多模态效果。

# 模型细节
![vlmo.png](./vlmo.png)
* `dual encoder`: 分别`encode`图像和文本，常见于检索任务。`fusion encoder`：同时`encode`图文对，其目的是模态更好的融合。

## 输入表征
* 图像表征：由于输入使用的模型是`vit`，所以处理方式是标准的`path`处理。$V_{image} = V_{embedding} + V_{pos} + V_{type}$.这里的$V_{type}$ 指的是输入类型：图像 `or` 文本。
* 文本表征：由于文本`encoder` 使用的是`BERT`，所以此处使用的也是 `wordPiece`。$V_{text} = V_{embedding} + V_{pos} + V_{type}$.
* 图像文本表征：$V=[ V_{text},V_{image}]$

## MoME
* 详细图表示如下：
![vlmo_2.png](./vlmo_2.png)
* 公式表示如下：
![vlmo_1.png](./vlmo_1.png)

* 图中的`MoME-FFN`，共有三种类型：`V-FFN`,`L-FFN`,`VL-FFN`.其中`V-FFN` 只有输入均是图像的时候使用，`L-FFN` 只有输入均是文本的时候使用，`VL-FFN` 当输入是两个模态的时候使用，而且分别经过V-FFN和L-FFN 获取特征以后，再经过`VL-FFN` 获取模态之间的交互。

## 损失
预训练任务有3个：
* 图文对比损失
* MLM
* ITM

## Stagewise Pre-Training
![vlmo_3.png](./vlmo_3.png)
* 只使用 `image` 数据训练模型，调整方式与`BEiT`相同，我们直接使用了`BEiT`模型参数初始化`attention`模块和`expert`参数。
* 只使用 `text` 数据训练模型，此时冻结了 `attention` 模块的参数，使用`MLM `优化调整 `文本expert` 的参数。 
* 使用`image-text pair` 训练整个模型

# 模型实验
## 数据集
`Conceptual Captions (CC) , SBU Captions , COCO and Visual Genome (VG)  datasets. There are about 4M images and 10M image-text pairs in the pre-training data`
## 下游任务
### Vision-Language Classification
取最后`[T_CLS]`作为输入表征，后再接入` task-specific`分类层。
* Visual Question Answering (VQA)
* Natural Language for Visual Reasoning (NLVR2)

![vlmo_4.png](./vlmo_4.png)

#### Vision-Language Retrieval
将`VLMo`作为 `dual encoder`使用，分别计算两个塔的输出表征。
![vlmo_5.png](./vlmo_5.png)