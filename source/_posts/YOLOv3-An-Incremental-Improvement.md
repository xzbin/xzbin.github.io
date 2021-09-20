---
title: 'YOLOv3: An Incremental Improvement'
categories:
  - [图像]
  - [论文阅读]
  - [目标检测]
date: 2021-06-13 22:51:48
tags:
  - YOLOv3
  - 目标检测
---


<div align='center' ><font size='5'>Abstract</font></div>

* 该文章属于YOLO系列模型的第三版, 相比 `YOLO9000`模型稍微大了一些,但是更为精确。速度相比SSD 也更快。``` At 320 × 320 YOLOv3 runs in 22 ms at 28.2 mAP, as accurate as SSD but three times faster```

<div align='center' ><font size='5'>Bounding Box Prediction</font></div>

* 与`YOLO9000 `保持一致，也是通过`kmeans` 的方式获取`anchors`。

* 使用`xywh`的方式标识bbox，其中预测方式与YOLO9000保持一致，按照如下公式：

  * $$
    \begin{align*}
    & b_x = \sigma(t_x) + c_x \\
    & b_y = \sigma(t_y) + c_y \\ 
    & b_w = p_we^{t_w} \\
    & b_h = p_he^{t_h}
    \end{align*}
    $$
  
    

<div align='center' ><font size='5'>Class Prediction</font></div>

