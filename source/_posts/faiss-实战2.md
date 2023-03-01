---
title: faiss 实战
date: 2023-02-23 09:36:59
tags: 
    - [faiss]
categories: 
    - [工程]
---

## faiss召回CPU代码
```python
import numpy as np
 
d = 64  # dimension
nb = 100000  # database size
nq = 10000  # nb of queries
np.random.seed(1234)  # make reproducible
xb = np.random.random((nb, d)).astype('float32')
xb[:, 0] += np.arange(nb) / 1000.
xq = np.random.random((nq, d)).astype('float32')
xq[:, 0] += np.arange(nq) / 1000.
 
ids = np.random.randint(0, 5, 100000)  # 生成ids
ids = ids.astype('int')
import faiss  # make faiss available
 
index2 = faiss.IndexFlatL2(d)  # build the index
#index2 = faiss.IndexIDMap(index)
 
print(index2.is_trained)
index2.add(xb)  # add vectors to the index
#index2.add_with_ids(xb, ids)
print(index2.ntotal)
import time
while True:
    start_time = time.time()
    k = 4  # we want to see 4 nearest neighbors
    D, I = index2.search(xb[:5], k)  # sanity check
    end_time = time.time()
    print(f"costTime: {end_time - start_time}")
    #print(xb.shape)
    #print('I', I)
    #print('D', D)
```
## faiss召回单GPU代码
```python
import numpy as np
import os
 
os.environ["CUDA_VISIBLE_DEVICES"] = "3"
 
d = 64  # dimension
nb = 100000  # database size
nq = 10000  # nb of queries
np.random.seed(1234)  # make reproducible
xb = np.random.random((nb, d)).astype('float32')
xb[:, 0] += np.arange(nb) / 1000.
xq = np.random.random((nq, d)).astype('float32')
xq[:, 0] += np.arange(nq) / 1000.
 
ids = np.random.randint(0, 5, 100000)  # 生成ids
ids = ids.astype('int')
import faiss  # make faiss available
 
index2 = faiss.IndexFlatL2(d)  # build the index
res = faiss.StandardGpuResources()  # use a single GPU
gpu_index_flat = faiss.index_cpu_to_gpu(res, 0, index2)
 
#index2 = faiss.IndexIDMap(index)
 
#print(index2.is_trained)
gpu_index_flat.add(xb)  # add vectors to the index
#index2.add_with_ids(xb, ids)
print(gpu_index_flat.ntotal)
 
import time
while True:
    start_time = time.time()
    k = 4  # we want to see 4 nearest neighbors
    D, I = gpu_index_flat.search(xb[:5], k)  # sanity check
    end_time = time.time()
    print(f"costTime: {end_time - start_time}")
    #print(xb.shape)
    #print('I', I)
    #print('D', D)
```
## faiss召回多GPU代码
```python
import numpy as np
import os
 
os.environ["CUDA_VISIBLE_DEVICES"] = "4,5,6"
 
d = 64  # dimension
nb = 100000  # database size
nq = 10000  # nb of queries
np.random.seed(1234)  # make reproducible
xb = np.random.random((nb, d)).astype('float32')
xb[:, 0] += np.arange(nb) / 1000.
xq = np.random.random((nq, d)).astype('float32')
xq[:, 0] += np.arange(nq) / 1000.
 
ids = np.random.randint(0, 5, 100000)  # 生成ids
ids = ids.astype('int')
import faiss  # make faiss available
 
index2 = faiss.IndexFlatL2(d)  # build the index
#res = faiss.StandardGpuResources()  # use a single GPU
#gpu_index_flat = faiss.index_cpu_to_gpu(res, 0, index2)
 
gpu_index_flat = faiss.index_cpu_to_all_gpus(index2)
 
#index2 = faiss.IndexIDMap(index)
 
#print(index2.is_trained)
gpu_index_flat.add(xb)  # add vectors to the index
#index2.add_with_ids(xb, ids)
print(gpu_index_flat.ntotal)
 
import time
while True:
    start_time = time.time()
    k = 4  # we want to see 4 nearest neighbors
    D, I = gpu_index_flat.search(xb[:5], k)  # sanity check
    end_time = time.time()
    print(f"costTime: {end_time - start_time}")
    #print(xb.shape)
    #print('I', I)
    #print('D', D)
```
