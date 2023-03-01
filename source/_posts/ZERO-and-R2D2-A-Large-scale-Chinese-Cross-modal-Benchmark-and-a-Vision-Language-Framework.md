---
title: >-
  ZERO and R2D2: A Large-scale Chinese Cross-modal Benchmark and a
  Vision-Language Framework
date: 2023-03-01 09:09:40
tags:
    - 多模态
    - ITR
    - R2D2
categories:
  - [论文阅读]
---
事先声明我个人对该文章提出的模型架构并不是很推崇，因为其设计不够优美，有点过于复杂了。
***
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