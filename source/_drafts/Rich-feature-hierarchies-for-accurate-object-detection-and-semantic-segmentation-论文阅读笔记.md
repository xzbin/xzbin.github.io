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

更加直观的可以通过下面这张图片来显示：系统框架如下：

![rcnn_overview](rcnn_overview.png)

1. 模型的整个输入是一张图片
2. 对这张图片进行区域划分，得到很多的region proposal。大约2k，这里使用的方法是 selective mothed.
3. 对一个region ，使用CNN进行提取
4. 使用SVM 进行分类。



Alex net 网络的输入是 227 * 227 pixel size。但是我们得到的region propsals 是不规则的，并不符合这个要求。

作者采用了一个最简单的方式：忽略图片的大小和长宽比，这部分没看懂（图像的dilate）。最终是转换成





测试阶段：

测试阶段，按照上面的步骤一次进行，然后应用一个 *Greedy* *non-maximum* *suppression*  方法来选择区域。

训练：

1. 作者采用的是预训练好的模型，
2. 调优：此时将alex net 最后一层softmax去掉，然后加入 新的softmax。如果需要分类是N类，则最后softmax分类是N+1类，有一类是背景；关于正负样本的选择，并不是将训练集中的数据，直接用于调优，而是先将一张图片进行区域划分得到一系列proposal，然后将得到的proposal与真实region 值计算IoU，大于0.5被选中为正值，其余为负值。但是在训练的时候，学习率设置为0.001，每个batch128个样本由32个正样本，96个背景样本组成。
3. 分类：此时我们选择的样本是IoU大于0.3的我们认为是正样本，否则负样本。使用SVM进行训练，此时训练数据过大，不能全部放入内存，此时采用的是 standard hard negative mining method。一次循环就可以到达最优。