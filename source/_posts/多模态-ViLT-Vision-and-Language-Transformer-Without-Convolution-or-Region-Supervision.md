---
title: ViLT Vision-and-Language Transformer Without Convolution or Region Supervision
categories:
  - [论文阅读]
date: 2023-02-07 18:59:21
tags:
  - 多模态
  - ViLT
---

# 文章idea
* 文章认为`heavy image-encoder`无论在训练还是在真实的使用场景都是不好的，因此作者使用了足够`light` 并且`fast`的方式：直接怼`path`进行`linear projection`。

# 文章贡献
* 提出了非常简单的多模态模型架构 `ViLT`。

# 模型细节
![model_overview](./model_overview.png)
* `Vision-and-Language Pretraining (VLP)` 在`vision`和`NLP`下游任务中表现非常好。目前的`VLP`方法非常依赖于图像特征提取，其中大部分涉及`region supervision`。

* 我们提出了一个最小的`VLP`模型，`Vision-and-Language Transformer (ViLT)`。其中，`visual inputs`的处理也被简化无卷积方式。

## Visual Embedding Schema
* `visual embedding`是现有的`VLP`模型的瓶颈，原因是：现有的`VLP`模型在`text embedder`上面是相同的，在` visual embedders`是不同的。
* `Region Featur`: `VLP`模型中占主导地位，它们来自预训练好的目标检测器，比如：基于`Visual Genome (VG)`数据库训练的`Faster R-CNN`。`RPN`网络会产生数千个`RoI`，`Non-maximum suppression`会减少`RoI`的数据量到`1~2K`。再经过一些池化操作(比如` RoI Align`)，`RoI heads`将`ROI`提取`region features`。`NMS`将`feature`数量减少到100一下。但是非并行的`NMS`是一个严重的运行瓶颈。当类别数变大的时候，这个问题会变得更为严重。

* `grid features`：直接使用`grid features`被` VQA-specific models`提出。
`Pixel-BERT`也是使用`grid features`的`VLP`模型。
`grid features`会被直接输入模态交互层。`grid features`
* 但是`grid features`并不是首选，`CNN`非常`重`，非常消耗计算资源。

* `Patch Projection`: 为了减少开销，我们使用了最简单的`embedding`方案：对`image patch`的线性映射。`path projection embedding`是由`ViT`引入，首先用于分类任务。`Patch Projection`将`image embedding`大大简化为`textual embedding`。

## 模型公式表示
* `ViLT`具有非常小的视觉`embedding pipeline`，并遵循单流方法，因此`ViLT`可以说是的最简单架构的`VLP`模型。

* 我们初始化`interaction transformer`层使用的是`ViT`的权重，而且不是`BERT`，主要是因为我们想利用其处理视觉特征的能力。
![vlp_progress](./vlp_progress.png)

* `VIT`的`encoder`包含了多个堆叠的多头attention(MSA)和一个MLP层。`LN`层的位置再`MSA`和`MLP`之前，这与`BERT`不同。


* 文本输入 $t\in R^{L\*|V|}$ 通过 word embedding 矩阵 和 position矩阵 映射成`H`维度。
![text-emb-H](./text-emb-H.png)

* 图像输入 $ I\in R^{C\*H\*W}$ 通过 word embedding 矩阵 和 position矩阵 映射成`H`维度。
![image-emb-H](./image-emb-H.png)

## 损失函数
* `ViLT`有2个目标函数：`masked language modeling (MLM)` and `image text matching (ITM)`。

* `ITM`: 以0.5概率随机替换图像，生成负样本。另外作者还设计了`WPA`损失用来计算word和patch的对齐分数。
* `MLM`: 预测被mask掉的 token。与`BERT`的MLM损失一样。

# 模型实验
## 数据
![vilt.png](./vilt.png)
该模型只评估了 `V-L` 任务。

## 下游实验
此处只考虑 retrieval 性能
### retrieval
文章分别统计了模型在`zero-shot`与`fintune`后的性能表现，由于模型结构的特殊性，所以再检索任务的时候，不能使用`cos 相似度`的方式。只能使用`ITM head`的输出。
#### Zero-shot 性能
![vilt_2.png](./vilt_2.png)

#### finetune 性能
![vilt_3.png](./vilt_3.png)