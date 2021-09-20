---
title: faiss 常用方式
date: 2021-09-19 21:12:29
tags: [faiss]
categories: 
  - [工程化]
---

## faiss 简介

## 安装方式

```python
# cpu 版本
pip install faiss-cpu
```

##  Index 介绍

* 以`IndexFlatL2`和`IndexFlatIP`为例展示其使用方式。其他的`index`使用参考[链接](https://github.com/facebookresearch/faiss/wiki/Faiss-indexes)。

| Method                         | Class name    | `index_factory` | Main parameters | Bytes/vector | Exhaustive | Comments                                           |
| ------------------------------ | ------------- | --------------- | --------------- | ------------ | ---------- | -------------------------------------------------- |
| Exact Search for L2            | `IndexFlatL2` | `"Flat"`        | `d`             | `4*d`        | yes        | brute-force                                        |
| Exact Search for Inner Product | `IndexFlatIP` | `"Flat"`        | `d`             | `4*d`        | yes        | also for cosine (**normalize vectors beforehand**) |

* 常用的距离度量`cosine `，但是使用之前需要进行`normalize `，否则不会生效。

## 带有id的查询方案

```python
# faiss 增加ids
import numpy as np
import faiss  # make faiss available


dimension = 64
num_train = 1000
num_val = 10

train_embeeding = np.random.random((num_train, dimension)).astype('float32')
query_embedding = np.random.random((num_val, dimension)).astype('float32')
train_ids = np.random.randint(0, 5, num_train).astype('int')  # 生成ids

faiss.normalize_L2(train_embeeding)
faiss.normalize_L2(query_embedding)

index = faiss.IndexFlatIP(dimension)  # build the index
index2 = faiss.IndexIDMap(index)
print(index.is_trained)
index2.add_with_ids(train_embeeding, train_ids)
print(index.ntotal)

k = 4  # we want to see 4 nearest neighbors
D, I = index2.search(query_embedding[:5], k)  # sanity check
print('I', I)
print('D', D)
```





## 常用链接

1. [faiss github](https://github.com/facebookresearch/faiss)
2. [faiss index 文档](https://github.com/facebookresearch/faiss/wiki/Faiss-indexes)
