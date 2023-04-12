---
title: >-
  CAN-NER: Convolutional Attention Network for Chinese Named Entity
  Recognition
tags: 
    - [CAN-NER]
categories: 
  - [NER]
  - [论文阅读]
---

* 原始论文: [CAN-NER: Convolutional Attention Network for Chinese Named Entity Recognition](https://arxiv.org/abs/1904.02141)
* 官方代码：[CAN-NER](https://github.com/microsoft/vert-papers/tree/master/papers/CAN-NER)

# 文章idea
* 由于`英文`自带分隔符，所以其分词非常容易；但是`中文`不同，缺少分隔符，所以不能很容易进行分词。
* 很多`中文NER`模型都需要额外的词典用以提高性能。但是这样做有很多问题：`OOV`，` model size`，`获取干净而且大量的标注数据很困难`。
*  `character embedding`的信息是有限的，比如“拍”在“球拍”和“拍卖”中的含义是不同的。
*  因此我们提供了`基于CNN的 attention`层，可以从字符序列中抽取局部的内容特征。
# 文章主旨
*  提出了基于`基于CNN的 attention`的模型：`CAN-NER`。
# 模型架构
# 模型实验