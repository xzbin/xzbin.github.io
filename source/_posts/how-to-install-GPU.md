---
title: 如何在服务器上配置GPU环境
categories:
  - 工程
date: 2023-03-30 16:36:39
---

* 简要记录服务器中`GPU` 环境搭建的具体流程

# 安装过程

## 升级 gcc
```
sudo yum install centos-release-scl
sudo yum install devtoolset-7
scl enable devtoolset-7 bash
```

## 下载驱动
```
https://www.nvidia.cn/Download/index.aspx?lang=cn
选择对应的GPU型号，下载驱动
```

## 关闭 nouveau
``` 
sudo vim /etc/modprobe.d/blacklist-nouveau.conf
在其中写入
blacklist nouveau
options nouveau modeset=0
```
 
## 重启服务器
```
sudo reboot
```

## 安装驱动
```
sh NVIDIA-Linux-x86_64-515.65.01.run
```
## 更改显卡persistenced模式
```
sudo nvidia-persistenced --persistence-mode
```
