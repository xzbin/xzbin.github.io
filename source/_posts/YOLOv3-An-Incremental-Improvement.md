---
title: 'YOLOv3: An Incremental Improvement'
categories:
  - [论文阅读]
date: 2021-06-13 22:51:48
tags:
  - YOLOv3
  - 目标检测
---


<!-- <div align='center' ><font size='5'>Abstract</font></div> -->

* 该文章属于YOLO系列模型的第三版, 相比 `YOLO9000`模型稍微大了一些,但是更为精确。速度相比SSD 也更快。``` At 320 × 320 YOLOv3 runs in 22 ms at 28.2 mAP, as accurate as SSD but three times faster```
* YOLOv3 与其他检测器的性能对比，更快更好：
![inference_time](./inference_time.png)
![better_performance](./better_performance.png)
<!-- <div align='center' ><font size='5'>Bounding Box Prediction</font></div> -->
* 与`YOLO9000 `保持一致，也是通过` dimension clusters` 的方式获取`anchors`。此处的`anchors`可理解成一组预定义的框。
* 网络针对每个`bounding box`预测了4个坐标: $t_x,t_y,t_w,t_h$。
* `If the cell is offset from the top left corner of the image by (cx, cy) and the bounding box prior has width and height` $p_w, p_h$，cell与bbox的关系，如下图所示：![cell_bbox](./cell_bbox.png)
* 使用`xywh`的方式标识bbox，其中预测方式与 `YOLO9000` 保持一致，按照如下公式：
$$
    \begin{align*}
    & b_x = \sigma(t_x) + c_x \\
    & b_y = \sigma(t_y) + c_y \\ 
    & b_w = p_we^{t_w} \\
    & b_h = p_he^{t_h}
    \end{align*}
$$
* 训练过程中，`bbox`使用平方差损失，` object`使用逻辑损失。
*  `our system only assigns one bounding box prior for each ground truth object` 如果多个先验的bbox超过`IOU`阈值，只取其中最优的一个。此处阈值为`0.5`。
* 在类别预测的时候，使用的是` binary cross-entropy loss`. 原因是 `softmax`具有排他性。`Using a softmax imposes the assumption that each box has exactly one class which is often not the case. A multilabel approach better models the data`
* YOLOv3的输出有3个分支【尺寸】，其中每个分支的输出尺寸是：`N × N × [3 ∗ (4 + 1 + 80)]` 其中：`3：number of bounding box prior, 4：bounding box offsets, 1：objectness prediction, 80： class predictions`.
* 网络结构不同于`YOLOv2, Darknet-19`,使用的是新的结构`Darknet-53`。![](./network_framework.png)
* `Darknet-53`相比其他的网络`ResNet-101 or ResNet-152`结构更为有效:
![darknet_53_backbone](./darknet_53_backbone.png)

<!-- AP50，AP60，AP70……等等指的是取detector的IoU阈值大于0.5，大于0.6，大于0.7…
https://blog.csdn.net/weixin_38145317/article/details/106405245
 -->


<!-- <div align='center' ><font size='5'>Class Prediction</font></div> -->

