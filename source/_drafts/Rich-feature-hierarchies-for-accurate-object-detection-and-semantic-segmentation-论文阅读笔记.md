---
title: >-
  Rich feature hierarchies for accurate object detection and semantic
  segmentation 论文阅读笔记
categories: [object detection]
---



本篇论文提出的object detection 系统包含三个模块：

1. 对图片进行切割，得到一系列region proposals，大约是2k，这里采用的方法是 [selective search](http://www.huppelen.nl/publications/selectiveSearchDraft.pdf)
2. 通过CNN网络对1 中得到的region proposals 进行特征抽取，得到固定长度的特征，这里采用的是Alex Net ，去掉最后的softmax层，将最后一层的输出（4096维）的作为特征。
3. 使用SVM进行分类。

系统框架如下：

![rcnn_overview](rcnn_overview.png)

1. 模型的整个输入是一张图片
2. 对这张图片进行区域划分，得到很多的region proposal。大约2k，这里使用的方法是 selective mothed.
3. 对一个region ，使用CNN进行提取
4. 使用SVM 进行分类。







