---
title: GLM-130B 模型环境搭建流程
tags:
  - [GLM-130B]
  - [GLM]
  - [AIGC]
categories:
  - [论文阅读]
  - [工程]
date: 2023-03-30 16:36:39
---


* 由于[GLM-130B](https://github.com/THUDM/GLM-130B) 工作非常`soild`，于是尝试部署验证模型效果
* 其说明非常详细，基本上按照上面的要求来即可。有一些小`trick`，于是记录如下：

# 安装过程

1. 先配置虚拟环境，文档中要求`python`版本`3.9+`，这里选择`3.9.16`
```
conda create -n llm python=3.9.16
```
2. 安装 `cuda`过程略，服务器安装版本`11.6`
3. 安装 `torch`，查询[网址](https://pytorch.org/get-started/previous-versions/)
```
python3 -m pip install torch==1.12.0+cu116 torchvision==0.13.0+cu116 torchaudio==0.12.0 --extra-index-url https://download.pytorch.org/whl/cu116 -i http://mirrors.aliyun.com/pypi/simple/  --trusted-host mirrors.aliyun.com
```

4. 安装`apex`，具体参见[here](https://github.com/NVIDIA/apex/#linux)，但是过程中，会出现 `torch`与 `cuda` 版本不匹配问题，上有人建议[`remove this check`](https://github.com/NVIDIA/apex/pull/323#discussion_r287021798)，不得不说很硬核，然后试了下，居然成功了，具体注释的代码行。注：这一步时间比较长。
![glm_1.png](./glm_1.png)
5. 安装`DeepSpeed`
```
python3 -m pip install deepspeed -i http://mirrors.aliyun.com/pypi/simple/  --trusted-host mirrors.aliyun.com
```
6. 安装`SwissArmyTransformer `
```
python3 -m pip install SwissArmyTransformer -i http://mirrors.aliyun.com/pypi/simple/  --trusted-host mirrors.aliyun.com
```

7. 安装 `requirements.txt`
```
python3 -m pip install -r requirements.txt -ihttp://mirrors.aliyun.com/pypi/simple/  --trusted-host mirrors.aliyun.com
```

8. 下载模型文件，此处需要填写调查问卷，然后通过邮件获取到 `url.txt`。
9. 邮件中建议通过 `aria2`下载模型，所以先安装`aria2`，[下载链接](https://github.com/aria2/aria2/releases/tag/release-1.36.0):
```
yum install aria2 -y
```
9. 根据邮件提示下载 `checkpoint`。由于有`60`个子文件，所以下载的时候可能需要多次执行。
```
aria2c -x 16 -s 16 -j 4 --continue=true -i urls.txt
```
