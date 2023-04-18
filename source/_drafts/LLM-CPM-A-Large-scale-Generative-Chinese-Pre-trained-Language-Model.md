---
title: 'CPM: A Large-scale Generative Chinese Pre-trained Language Model'
tags: 
    - [CPM]
    - [AIGC]
categories: 
  - [论文阅读]
  - [LLM]
  - [LM]
---

* 原始论文: [CPM: A Large-scale Generative Chinese Pre-trained Language Model](https://arxiv.org/abs/2012.00413)
* 官方代码: [CPM-Generate](https://github.com/TsinghuaAI/CPM-Generate)

# 文章idea
* `GP3`在多个`NLP`任务中表现非常出色，但是由于其训练数据中大部分均是中文，所以其在中文任务的表现是有限的，而且是其并未开源。
* 因此本文提出`Chinese Pre-trained Language Model (CPM)`，该模型是基于生成式的预训练模型。
# 文章主旨
* 提出了中文自回归的语言模型，使用生成式的预训练，2.6B参数。
* 基于分词构建了新的词典；为了稳定的训练，batch size 被设定是 `3072`。

# 模型细节
## Vocabulary Construction
* 之前的中文预训练模型均是基于`character`，缺少单词的信息，会导致信息丢失，为了解决这个问题，作者重新组织了词典，加入了词的内容。
## Training Strategy
* 由于单词信息的稀疏性，所以作者使用了比`GPT-3`大两倍的`batch-size`。

## Data Processing

## Pre-training Details

# 模型实验