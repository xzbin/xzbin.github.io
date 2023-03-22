---
title: learning transferable visual models from natural language supervision
date: 2023-01-04 13:11:49
categories:
  - [论文阅读]
  - [多模态]
tags:
  - CLIP
  - 图像表征
---

* 原始论文: [learning transferable visual models from natural language supervision](http://proceedings.mlr.press/v139/radford21a)



严格意义上来讲，这不算是多模态论文，最起码说不是专门以多模态为`topic` 而写的论文。
# 文章idea
* 不使用标注好的数据[比较难获得，大量的人工清洗和标注]，直接使用<原始文本、图像>就可以获得不错的图像表征。

# 文章主旨
* 提出了`CLIP`: Contrastive Language-Image Pre-training

# 模型细节
![clip-model](./clip-model.png)

* 相对来说，CLIP 模型结构非常简单。 vision 部分可以使用`ResNet`也可以使用`Vit`结构
* `batch size` 一定要大，文中是 32,768。
* `loss`是： `Contrastive loss`

# 模型训练过程
![train_model](./train_model.png)

