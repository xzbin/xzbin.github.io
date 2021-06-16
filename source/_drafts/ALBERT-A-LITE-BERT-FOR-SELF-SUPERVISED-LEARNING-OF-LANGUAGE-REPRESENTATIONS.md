---
title: 'ALBERT: A LITE BERT FOR SELF-SUPERVISED LEARNING OF LANGUAGE REPRESENTATIONS'
categories: [NLP]

---

模型尺寸的不断变大对训练、和使用都带来了非常大的负担。为了解决这些问题，作者提出了两项参数减少技术，减少了内容的消耗、加速了训练的速度。除此作者使用`SOP`任务代替了`NSP`。

`ALBERT`模型的backbone使用的`transformer`的`encoder`层，这部分与`BERT`保持一致的。具体的参数标记如下：`embedding size E, the number of encoder layers as L, and the hidden size as H. we set the feed-forward/filter size to be 4H and the number of attention heads to be H/64`。这里`BERT`里面的参数如下：`E:512; L:12;H: 512` 。

ALBERT的三项贡献：

Factorized embedding parameterization



Cross-layer parameter sharing



Inter-sentence coherence loss



