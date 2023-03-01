---
title: 'UNITER: learning universal image-text representations'
date: 2023-02-24 20:41:28
tags:
    - 多模态
categories:
  - [论文阅读]
---

# 文章主旨
* 提出了新的多模态框架`UNITER`，而且在多个`V+L`任务中取得`SOTA`
* 作为2020年的多模态论文，在图像特征提取方面还使用的是检测的框架。因为`VIT`是2021年才出现。

# 模型结构

![UNITER](./uniter.png)

## Image Embedder

* `Image Embedder` 使用了 `Faster R-CNN`。针对每个`region`抽取特征(`pooled ROI features`)，每个`region`的`Location`特征使用7维的特征进行描述: `[x1, y1, x2, y2, w, h, w ∗ h](normalized top/left/bottom/right coordinates, width, height, and area.`；最终将 `visual`特征和`Location`特征结合在一起。
* `Our Faster R-CNN was pre-trained on Visual Genome object+attribute data`

## Text Embedder

* `Text Embedder` 与`BERT`相同，使用`WordPiece`提取句子中的`token`。将`token embedding` 与 `position embedding` 结合在一起。
* 最终图像与文本特征交叉层使用的 `transformer-Encoder`。

## 损失
* 该框架有三个损失函数：`Masked Language Modeling (MLM), Image Text Matching (ITM), and Masked Region Modeling (MRM, with three variants)`
* `MLM`: 无需过多赘述
* `ITM`：无需过多赘述
* `MRM`: 随机`mask` 15%的`region`，其值用`0`代替，与文本用离散`[mask]`表示不同。


# 模型试验
* 数据集共4个：`COCO, Visual Genome, Conceptual Captions, and SBU Captions`
* 基于预训练好的模型，在task中进行`finetune`，共评估了6个任务，2种模型：`UNITER-base with 12 layers ` and and `UNITER-large with 24 layers`。
![uniter_1.png](./uniter_1.png)
