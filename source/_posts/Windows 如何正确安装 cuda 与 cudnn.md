---
title: Windows 如何正确安装 cuda 与 cudnn
date: 2022-08-26 17:02:08
tags:
---

# Windows 如何正确安装 cuda 与 cudnn

首先要了解到的信息是，你的 Nvidia 显卡到底支持多高版本的 CUDA? 在系统状态栏中找到 `NVIDIA 控制面板`，查看 `[帮助]-[系统信息]-[组件]-[NVCUDA64.DLL]` 查看后面的版本，你需要安装的 CUDA 不得高于此版本。 


![image-20220826191251461](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220826191251461.png)


但是这里要注意的是，并不是你的显卡型号决定了你支持的 CUDA 版本，而是你的显卡和显卡驱动共同决定的。想要更新你的显卡驱动，可以尝试使用 NVIDIA GeForce Experience 进行更新，之后再在 `NVIDIA 控制面板` 查看支持的 CUDA 版本。


确定要下载的版本后，在此网站 [CUDA Toolkit Archive](https://developer.nvidia.cn/cuda-toolkit-archive) 找到你需要的版本，我这里选择了 CUDA 11.1.1 的版本，并且按照下图选择了对应我系统的安装包：



![image-20220826192316090](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220826192316090.png)



打开安装包，安装过程中选择自定义安装，然后查看下图中的项目，如果当前版本大于新版本则将此项取消勾选，反之则可以勾选。



![image-20220826192856077](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220826192856077.png)



安装完成后打开 Powershell 输入 `nvcc -V` 查看是否成功安装：

![image-20220826210836346](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220826210836346.png)



然后是 cuDNN(针对于神经网络应用加速的库)，进入网站 [cudnn-download](https://developer.nvidia.cn/rdp/cudnn-download) ，这里需要先注册一个 NVIDIA 账号，然后选择对应的版本进行下载。

将下载的压缩包解压，将里面三个文件夹直接放入之前安装的 CUDA 目录中。



![image-20220826211547617](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220826211547617.png)



验证一下是否安装成功，打开 Powershell 输入 `nvidia-smi`



![image-20220826211925113](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220826211925113.png)


可以看到安装是成功的