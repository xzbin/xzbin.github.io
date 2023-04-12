---
title: 'GLM-130B: AN OPEN BILINGUAL PRE-TRAINED MODEL'
tags: 
    - [GLM-130B]
    - [GLM]
    - [AIGC]
categories: 
  - [论文阅读]
  - [LLM]
  - [LM]
---

* 原始论文: [GLM-130B: AN OPEN BILINGUAL PRE-TRAINED MODEL](https://arxiv.org/abs/2210.02414)
* 官方代码：[glm-130b](https://github.com/thudm/glm-130b)

# 文章idea
* `LLM` 在`zero-shot`和`few-shot`方面非常有吸引力；`GP3[175B]`开创了`100B-scale LLMs`研究的先河，但是其没有开源。
* 我们的目的是训练一个开源的，高精度的`100B-scale model`：`GLM-130B`。

# 文章主旨
* 提出了新的`100B-scale LLMs`：`GLM-130B`，该模型支持`英文`和`中文`。在A100集群`96 NVIDIA DGX-A100 (8×40G) GPU nodes`上训练近两个月的时间(2022年5月6日和7月3日)。
* 还总结了过程中的一些错误选项和失败经验。
* 没有使用`GPT`架构，我们使用了` General Language Model (GLM)`。

# 模型架构
* 由于模型`backbone`使用的是`GLM`，该架构详见[GLM: General Language Model Pretraining](/2023/03/29/LM-GLM-General-Language-Model-Pretraining/)

* `Layer Normalization`: 为了解决`Training instability`的问题，作者团队尝试了多种选择: `Pre-LN`、`Post-LN`、` Sandwich-LN` ，但是以上均没有产生很好的效果，最终使用了`Deep-Norm`。

* `Positional Encoding`：舍弃了`ALiBi`，转而使用了`Rotary Positional Encoding`。
* `FFN`：舍弃之前的激活函数`GeLU`，转而使用`GLU`进行代替。

## PRE-TRAINING SETUP
`GLM-130B` 除了使用原始的`GLM`自监督的损失之外，还使用了`multi-task learning`，其目的是下游任务的`zero-shot`性能。

* `Self-Supervised Blank Infilling`: 这部分就是使用的`GLM`自监督损失，`[MASK]`和`[gMASK]`:`[MASK]`表示比较短的`span`，其`MASK`长度符合`Poisson`分布`[λ = 3]`；`[gMASK]`表示 句子前缀作为`context`，剩下的部分表示为`[gMASK]`,其长度符合正态分布。这部分的预训练数据非常华丽：
  * `1.2T Pile English corpus`
  * `1.0T Chinese Wudao Corpora`
  * `250G Chinese corpora (including online forums, encyclopedia, and QA) we crawl from the web, which form a balanced composition of English and Chinese contents`
* `Multi-Task Instruction Pre-Training`: 使用了 74 prompted 数据集。

## PARALLEL STRATEGIES AND MODEL CONFIGURATIONS
`GLM-130B is trained on a cluster of 96 DGX-A100 GPU (8×40G) servers with a 60-day access`
* The 3D Parallel Strategy：
  * [`data parallelism`](https://dl.acm.org/doi/pdf/10.1145/79173.79181)和[`model parallelism`]()是大模型训练的基础操作。
  * 使用`PipeDream-Flush[DeepSpeed]`，其目的是减少训练时间和`GPU`内存浪费。
* GLM-130B Configurations


# 模型实验


# 参考文章
* [简单介绍：算力单位TOPS，GPU处理能力（TFLOPS/TOPS）
](https://imgtec.eetrend.com/blog/2021/100062210.html)
