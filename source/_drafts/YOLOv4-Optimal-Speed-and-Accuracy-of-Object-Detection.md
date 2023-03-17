---
title: 'YOLOv4: Optimal Speed and Accuracy of Object Detection'
categories:
  - [论文阅读]
date: 2021-09-19 16:55:15
tags:
  - YOLOv4
  - 目标检测
---

在提高`CNN`模型精度方面人们做了很多尝试`feature`。在 `YOLOv4` 模型里面使用了很多通用 `feature`， 包括：`WRC, CSP,CmBN, SAT, Mish activation, Mosaic data augmentation, CmBN, DropBlock regularization, and CIoU loss`。通过联合使用以上努力，模型取得了最新的结果：`43.5% AP (65.7% AP50) for the MS COCO dataset at a realtime speed of ∼65 FPS on Tesla V100`


YOLOv4性能与其他检测器的对比：
![yolov4_performace](./yolov4_performace.png)

# Object detection models
## 通用object detector 的框架细节
`object detector` 的框架结构一般由3部分组成：`backbone`，`neck`，`head`。
`backbone`：预训练好的模型，比如：`VGG, ResNet, ResNeXt or DenseNet`。
`head`：用来解码预测目标的`class`和 `bounding box`。
`neck`：非必要组件，主要用来进行特征组合，比如：`FPN、PAN、 BiFPN、NAS-FPN`。
![framework_details](./framework_details.png)

## 通用object detector 的框架结构

![object_detector_framework](./object_detector_framework.png)

#  Bag of freebies
## data augmentation
* 通过改变训练策略，使得增长模型精度的方案叫：`Bag of freebies`。
* 最常用的方法是：`data augmentation`。其本质上是增加输入图像的多样性，使得模型有更好的鲁棒性。像素内容变换（`Photometric Distortions`）和 空间几何变换（`Geometric Distortions`）是两个常见的数据增强方式。
  * 像素内容变换（`Photometric Distortions`）：我们调整亮度，对比度。图像的色相、饱和度和噪声
  * 空间几何变换（`Geometric Distortions`）：随机缩放、裁剪、翻转和旋转。

## object occlusion
* `random erase` and `CutOut`: 随机的选取图像中的矩形区域，并随机填入或补充值为零。
* `hide-and-seek` and `grid mask`: 随机或均匀地选择多个图像中的矩形区域，并将其替换为全0。相似的概念应用到`feature map`上，方法有：`DropOut`, `DropConnect`, `DropBlock`。
* 多个图像在一起做数据增强，`MixUp`:使用了两个图像乘以不同系数的相乘叠加，并以此调整标签。`CutMix`:它是将裁剪后的图像覆盖到其他图像的矩形区域，并根据混合区域的大小调整标签。
* `style transfer GAN`: 也经常用于数据增强，这种使用可以有效地减少CNN学习到的纹理偏差。







<!-- https://blog.csdn.net/mzpmzk/article/details/100161187?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_paycolumn_v3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1.pc_relevant_paycolumn_v3&utm_relevant_index=2 -->


