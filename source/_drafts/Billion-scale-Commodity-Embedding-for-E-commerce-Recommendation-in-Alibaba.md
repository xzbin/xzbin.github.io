---
title: Billion-scale Commodity Embedding for E-commerce Recommendation in Alibaba
date: 2022-02-06 18:38:44
tags:
    - CTR
categories:
  - [论文阅读]
---

该方案主要应用于淘宝直播平台，通过A/B测试，我们发现在线点击率（CTR）比之前淘宝广泛使用的基于协同过滤的方法有所提高。

推荐系统面临三大挑战：可扩展性、稀疏性和冷启动。我们提出的办法可以解决这些问题，这些方法基于一个众所周知的`graph embedding`框架：我们首先根据用户的行为历史构造一个商品图，学习所有商品的`embedding`。这些`embedding`被用于计算所有商品两两之间的相似性，然后在推荐过程中使用这些相似性。为了缓解稀疏性和冷启动问题，我们在`graph embedding`框架中加入了`side information`。我们提出了两种方法来整合商品的`embedding`和相应的`side information`。离线实验的结果表明，加入`side information`的方法优于不加入的方法。此外，我们还描述了`embedding`部署的平台，以及淘宝处理上亿规模数据的工作流程。

淘宝面临三大技术挑战：
* Scalability【可扩展性】：现有的许多推荐方法在较小规模的数据集【比如：数百万】上运行良好，但是在大的数据集【比如：十亿】上表现比较差。
* Sparsity【稀疏性】：用户往往只与少量商品进行交互，因此很难训练出准确的推荐模型，尤其是对于交互次数非常少的用户或项目。它通常被称为“稀疏性”问题
* Cold Start【冷启动】：在淘宝网，每小时都有数百万条新商品被持续上传。这些商品没有用户行为。预测用户对这些商品的偏好很有挑战性，这就是所谓的“冷启动”问题。

为解决以上问题，淘宝设计了一个二阶段的处理方案：`matching`和`ranking`。在`matching`阶段，我们为用户交互过的每个项目生成一组相似项目的候选集。在`ranking`阶段，我们训练了一个深度神经网络模型，根据每个用户的偏好对候选项进行排名。

本文主要集中在`matching`阶段，核心任务是根据用户行为计算所有商品之间的相似性。在获得商品的相似性后，我们可以生成候选项目集，以便在`ranking`阶段进一步个性化。为了实现这一点，我们从用户的行为历史构建一个商品图，然后应用最先进的`graph embedding`方法来学习每个商品的`embedding`，称为`Base Graph Embedding（BGE）`.

根据商品`embedding`向量的点积计算出的相似性来确定候选项目集。以前的工作中，基于`CF`的方法只考虑用户行为历史中的项目的共现，在我们的工作中，使用商品图中的随机游走，我们可以捕捉商品之间的高阶相似性。因此，它优于基于`CF`的方法。然而，在很少甚至没有交互的情况下学习商品的准确`embedding`仍然是一个挑战。为了解决这个问题，我们使用`side information`来增强`embedding`，称为` Graph Embedding with Side information（GES）`。例如，属于同一类别或品牌的商品应该在嵌入空间中更靠近。通过这种方式，我们可以在很少甚至没有交互的情况下获得商品更为精确的`embedding`。然而，在淘宝，有数百种类型的附加信息，比如类别、品牌或价格等。

不同的`side information`对学习`embedding`的贡献不同。因此，我们进一步提出了一种`side information`的`embedding`学习的加权机制，称为`Enhanced Graph Embedding with Side information (EGES)`

该论文的三个重要部分：
* 我们设计了一种有效的启发式方法，从淘宝10亿用户的行为历史中构建商品图。
* 我们提出了三种嵌入方法，BGE、GES和EGES。 `GES` 和 `EGES`对比 `BGE`效果要好。
* 我们在`XTensorflow`平台上部署了该`graph embedding`系统。


















