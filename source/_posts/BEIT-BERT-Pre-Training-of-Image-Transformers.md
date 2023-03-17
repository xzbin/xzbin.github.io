---
title: 'BEIT: BERT Pre-Training of Image Transformers'
date: 2023-03-01 20:27:30
tags:
    - 图像
    - backbone
    - dVAE
    - image clssification
    - segmentation
categories:
  - [论文阅读]
---
# 文章主旨
* 提出了一个新的视觉表征方案`BEiT`，基于自监督任务`MIM`:`masked image modeling`。


# 模型架构
![beit.png](./beit.png)
* `BEiT` 将图像提取`vector`, 主要是通过自监督的方式实现，主要思路是：利用`vector` 预测`MASK patch`
## Image Representations
* 
# 模型实验