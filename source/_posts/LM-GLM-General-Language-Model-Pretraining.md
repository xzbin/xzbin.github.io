---
title: 'GLM: General Language Model Pretraining'
tags:
  - - GLM
categories:
  - - 论文阅读
  - - LM
date: 2023-04-06 16:37:49
---


* 原始论文: [GLM: General Language Model Pretraining](https://aclanthology.org/2022.acl-long.26/)
* 官方代码：[GLM](https://github.com/THUDM/GLM)

# 文章idea
* 基于`unlabeled text` 训练的`LM`提高了很多NLP任务的`sota`。
* 一般来讲，存在的预训练框架有三种：(1) `autoregressive`: 比如`GPT[left-to-right language models]`，在长文本生成和`few-shot`能力方面非常成功；其不足是单向的`attention`机制，不能够完全获取内容之间的依赖关系。(2) `autoencoding`: 比如`BERT`，双向的`encoder`，其不足是此方案只适用于`NLU`任务，不适合做文本生成。(3) `encoder-decoder`：比如 `transformer`，其`encoder`部分是双向，`decoder`是单向`attention` 和 `cross attention`，其适合做一些`conditional generation`任务，`text summarization`和`response generation`。
* 作者认为没有一个统一的框架，适合所有的`NLP任务`。所以本文提出了一个基于`blank infilling`的 `GLM(General language Model)`。 

# 文章主旨

* 提出了新的语言模型：`GLM(General language Model)`

# 模型架构
![glm.png](./glm.png)

* `GLM`被训练基于`autoregressive blank infilling` 损失。
* 针对一条输入 $$x=[x_1,...,x_n]$$，随机采样多个`span` $${s_1,...,s_n} $$，将其都替换成`[MASK]`，这样就得到了新的输入$$x_{corrupt}$$，模型以`autoregressive`的方式预测每条`span`中缺失的`tokens`。
* 针对`spans`的顺序进行`shuffle`，为了获取其相互之间的独立性。
* 定义 $$Z_m$$ 是长度是`m`序列[1, 2, ..., m]的所有可能的排列。则定义预训练目标则是：
  $$\max\limits_{\theta}E_{z\sim{Z_m}}	\left[\sum\limits_{i=1}^{m}logp_{\theta}(s_{z_i}|x_{sorrupt}, s_{z<i})\right]$$
* 输入`x`被分成两部分，`Part A`是$$x_{corrupt}$$，`Part B`包含了`masked spans`，由于此处使用了`mask attention`，所以：
  *  `Part A tokens can attend to each other, but cannot attend to any tokens in B`.
  *  `Part B tokens can attend to Part A and antecedents in B, but cannot attend to any subsequent tokens in B`
* 使用了多任务损失：
  * `Document-level`: `span`的长度采样是原始句子长度的`50%–100%`。此举目的是长文本生成。
  * `Sentence-level`: `span`被限制在所有的句子中，而且采样覆盖`15%`的原始`token`，此举的目的是`seq2seq`任务。

* `GLM`针对`Transformer`做了一些调整：
  * 针对 `LN` 与 `residual connection`的顺序进行了交换。
  * 最终输出只使用了一个`linear layer`预测`token`
  * 使用了`GeLUs`代替`ReLU`.
* 由于使用了`blank infilling`任务，所以对使用了`2D positional encodings`，每个`token`编码两个`position id`，详情见上图。
  * 第一个`position id`是表示$$x_{corrupt}$$的位置。
  * 第二个`position id`是表示`intra-span`的`position`。


## Finetuning GLM
![glm2.png](./glm2.png)
* 由于`GLM`属于生成式的预训练任务，所以其`Finetune`方式也采用了`blank infilling`。具体来讲：`we convert the input text x to a cloze question c(x) via a pattern containing a single mask token`；举例来讲，输入: `{SENTENCE}. It’s really [MASK]`。输出：`good` or `bad`.
* `conditional generation`：`partA`是`context` + `[MASK]`; 自回归的方式生成`partB`。
* 有可以使用`GLM`进行`unconditional generation`。

# 模型实验

略