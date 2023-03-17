---
title: learning transferable visual models from natural language supervision
date: 2022-05-04 13:11:49
categories:
  - [论文阅读]
tags:
  - [多模态]
  - CLIP
---

* `CLIP`模型结构非常简单，与现有的预训练模型结合之后可以大大减少训练数据的需要，比如中文领域 `text encoder` 可以使用中文预训练模型，比如`chinese albert`，`image encoder`可以使用预训练好的`vit`。
* 该模型结构不仅仅在图文领域可以使用。也可以推广到其他方面，比如 `行业id` 与 `文本`，这个我是做过验证的，也是有效果的，后续可以整理一篇文章发出来。

# 文章主旨

`CLIP`: Contrastive Language-Image Pre-training

# 模型细节
![clip-model](./clip-model.png)

* 相对来说，CLIP 模型结构非常简单。 vision 部分可以使用`ResNet`也可以使用`Vit`结构。但是经过试验对比`Vit`结构会更好一些。
* minibatch size of 32,768。
* loss是： Contrastive loss

# 模型训练过程
![train_model](./train_model.png)

