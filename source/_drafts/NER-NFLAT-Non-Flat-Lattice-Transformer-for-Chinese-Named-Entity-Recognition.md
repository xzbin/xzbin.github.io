---
title: 'NFLAT: Non-Flat-Lattice Transformer for Chinese Named Entity Recognition'
tags: 
    - NFLAT
categories: 
  - [NER]
  - [论文阅读]
---

* 原始论文: [NFLAT: Non-Flat-Lattice Transformer for Chinese Named Entity Recognition](https://arxiv.org/pdf/2205.05832v3.pdf)
* 官方代码：[nflat4cner](https://github.com/codermusou/nflat4cner)

# 文章idea
* `中文NER`与`英文`不同，中文字之间没有分隔符，不能直接获取词，也正是由于这部分信息的缺失，增加了`中文NER`任务的难度。
* 最近几年提高`中文NER`任务的方法均是使用`外部数据`：词汇信息、字形信息、、句法信息和语义信息等等。`Flat-LAttice Transformer (FLAT)`是其中很流行的词扩展方法，但是其在计算复杂度和内存使用要求较高，为了解决这些问题，提出了新的模型架构：`NFLAT`。
# 文章主旨
* 提出了`non-flat-lattice`网络架构，以`InterFormer`为 `backbone`，命名为 `NFLAT`
* 使用了外部的标签，来帮助模型自动抑制噪声信息并发现正确的单词。
# 模型架构
# 模型实验
