---
title: torchFM 代码解读系列-综述
date: 2023-02-02 15:45:47
tags: 
    - 代码解读
categories: 
  - 工程
  - 代码解读
---

今天开一个新系列，`torchFM代码解读`，以加深对模型结构的理解。[代码仓库](https://github.com/forrestneo/pytorch-fm)，虽然仓库非常老了，但是依旧有借鉴意义。这个仓库只是半成品，很多地方不够完美，可以借鉴其实现思路，但是最好不要完全复用。

# [AFN](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/afn.py)
原始论文: [Attentional Factorization Machines: Learning the Weight of Feature Interactions via Attention Networks](https://arxiv.org/abs/1708.04617)

# [NFM](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/nfm.py)
原始论文: [Neural Factorization Machines for Sparse Predictive Analytics](https://arxiv.org/abs/1708.05027)


# [NCF](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/ncf.py)
原始论文: [Neural Collaborative Filtering](https://arxiv.org/abs/1708.05031)


# [FNFM](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/fnfm.py)
原始论文: [Field-aware Neural Factorization Machine for Click-Through Rate Prediction](https://arxiv.org/abs/1902.09096)








# [AFI](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/afi.py)
原始论文: [AutoInt: Automatic Feature Interaction Learning via Self-Attentive Neural Networks](https://arxiv.org/abs/1810.11921)

# [AFN](https://github.com/forrestneo/pytorch-fm/blob/master/torchfm/model/afn.py)
原始论文: [Adaptive Factorization Network: Learning Adaptive-Order Feature Interactions](https://arxiv.org/pdf/1909.03276.pdf)