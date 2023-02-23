---
title: faiss 介绍
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


## 常用链接

1. [faiss github](https://github.com/facebookresearch/faiss)
2. [faiss index 文档](https://github.com/facebookresearch/faiss/wiki/Faiss-indexes)
