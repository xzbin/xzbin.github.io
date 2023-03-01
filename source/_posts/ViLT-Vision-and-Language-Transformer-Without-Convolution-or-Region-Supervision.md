---
title: ViLT Vision-and-Language Transformer Without Convolution or Region Supervision
categories:
  - [论文阅读]
date: 2022-02-07 18:59:21
tags:
  - [多模态]
  - VILT
---

`Vision-and-Language Pretraining (VLP)` 在`vision`和`NLP`下游任务中表现非常好。目前的`VLP`方法非常依赖于图像特征提取，其中大部分涉及`region supervision`。

我们提出了一个最小的`VLP`模型，`Vision-and-Language Transformer (ViLT)`。其中，`visual inputs`的处理也被简化无卷积方式，与文本输入。

我们表明，`ViLT`比以前的`VLP`模型快60倍。对下游任务具有更好的性能。

作者提出了新`VLP`模型分类，基于以下两点:`1: `这两种模态是否具有相同水平的表达【在专用参数和/或计算方面的】。`2: `这两种模态是否会在一个深度网络中融合。

由此引出的4个原型，`(a):` `VSE++`和`SCAN`属于，其分别使用了分离的`image`和`text`的`encoder`，但是前者的`encoder`更为`重`一些，使用` dot products`或`shallow attention layers`表征两种模态的相似性；`(b):` `CLIP`也使用了分离的`encoder`，两模态的`encoder`是一样`重`，两个模态的特征向量依然使用`dot product`进行交互，`CLIP`在`zero-shot`图像到文本检索方面表现出色，但是在其他图文下游任务表现一般；`(c):`最近更多的`deep transformer`模型被用来融合两个模态的特征，此时卷积网络仍用来编码图像特征，因此占据了大部分计算量，所以与`(a):`一样，图像的`encoder`更为`重`一些；`(d):`文中的`ViLT`，图像的`encoder`变得更为`轻`，模型特征的模型层变得更为`重`。图示如下: ![vlp_four](./vlp_four.png)


## Modality Interaction
`Bugliarello et al. (2020)`将特征融合分成2类：1. 单流方法（`Visual-BERT`，`UNITER`），两种模态在输入层进行串联，共同进行后续操作；2. 双流方法（`ViLBERT`，`LXMERT`）两个模态并没有在输入级别串联。我们的融合模块采用单流方法，因为双流方法引入了额外的参数。

## Visual Embedding Schema
`visual embedding`是现有的`VLP`模型的瓶颈，原因是：现有的`VLP`模型在`text embedder`上面是相同的，在` visual embedders`是不同的。
`Region Featur`: `VLP`模型中占主导地位，它们来自预训练好的目标检测器，比如：基于`Visual Genome (VG)`数据库训练的`Faster R-CNN`。`RPN`网络会产生数千个`RoI`，`Non-maximum suppression`会减少`RoI`的数据量到`1~2K`。再经过一些池化操作(比如` RoI Align`)，`RoI heads`将`ROI`提取`region features`。`NMS`将`feature`数量减少到100一下。但是非并行的`NMS`是一个严重的运行瓶颈。当类别数变大的时候，这个问题会变得更为严重。

`grid features`：直接使用`grid features`被` VQA-specific models`提出。
`Pixel-BERT`也是使用`grid features`的`VLP`模型。
`grid features`会被直接输入模态交互层。`grid features`
但是`grid features`并不是首选，`CNN`非常`重`，非常消耗计算资源。

`Patch Projection`: 为了减少开销，我们使用了最简单的`embedding`方案：对`image patch`的线性映射。`path projection embedding`是由`ViT`引入，首先用于分类任务。`Patch Projection`将`image embedding`大大简化为`textual embedding`。

###  Model Overview
`ViLT`具有非常小的视觉`embedding pipeline`，并遵循单流方法，因此`ViLT`可以说是的最简单架构的`VLP`模型。

我们初始化`interaction transformer`层使用的是`ViT`的权重，而且不是`BERT`，主要是因为我们想利用其处理视觉特征的能力。
![vlp_progress](./vlp_progress.png)

`VIT`的`encoder`包含了多个堆叠的多头attention(MSA)和一个MLP层。`LN`层的位置再`MSA`和`MLP`之前，这与`BERT`不同。

详细的模型结构如下图所示：
![model_overview](./model_overview.png)

文本输入 $t\in R^{L*|V|}$ 通过 word embedding 矩阵 和 position矩阵 映射成`H`维度。
![text-emb-H](./text-emb-H.png)

图像输入 $ I\in R^{C*H*W}$ 通过 word embedding 矩阵 和 position矩阵 映射成`H`维度。
![image-emb-H](./image-emb-H.png)

###  Pretraining Objectives
`ViLT`有2个目标函数：`masked language modeling (MLM)` and `image text matching (ITM)`。

`ITM`: 以0.5概率随机替换图像，生成负样本。另外作者还设计了`WPA`损失用来计算word和patch的对齐分数。
`MLM`: 预测被mask掉的 token。与`BERT`的MLM损失一样。

### Whole Word Masking
mask掉整个单词。由于 `wordpiece`会将一个单词分成多个 `token`。所以`mask`的时候，需要将所有的token全部mask掉。
mask的概率是 15%.
### Image Augmentation
在 `finetuning` 的时候使用了RandAugment。除去了`color inversion` 和 `cutout`。

其他资料：
https://huggingface.co/docs/transformers/v4.16.2/en/model_doc/vilt
