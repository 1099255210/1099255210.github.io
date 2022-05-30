---
title: Ubuntu 20.04 与 win10 双系统安装笔记
date: 2020-09-30 12:39:08
tags:
---

# Ubuntu 20.04 与 win10 双系统安装笔记

## 安装前的认知准备

在安装之前先了解一下基本的安装知识以及需求，以下为我认知中所需要作的准备：

- 一个 8G 以上的u盘(作为启动盘)
- Ubuntu 20.04 系统镜像(从[Ubuntu官网](https://ubuntu.com/download/desktop)下载)
- 50G 及以上的磁盘空间

本人电脑型号是 ThinkPad T460s，原操作系统为 win10 2004 x64，BIOS 模式为 UEFI 引导，电脑仅有一块 256G 固态硬盘，无独立显卡，因此应该不会出现N卡驱动缺失的情况。

以上了解之后开始操作。

## 准备与安装全过程

首先，为系统分出足够空间。右键 **此电脑**，找到 **管理**>**存储**>**磁盘管理** ，右键磁盘，点击压缩卷。在这里我分出了 100G 的空间，应该足够轻度的使用。

然后开始制作启动盘。下载制作启动盘的软件 [rufus](http://rufus.ie/) 并打开，插入u盘，在软件中选择正确的u盘与 ubuntu 镜像， 其余项目并不需要修改，直接开始，提示信息可忽略。耐心等待10-20分钟即可制作完成。

接下来是 BIOS 的设置。重启电脑前，找到电脑电源设置，关闭快速启动；然后重启电脑，找到本电脑型号进入 BIOS 的快捷键，我的电脑是 DELETE；进入 BIOS 之后，将 Secure Boot 设置为 Disabled，再确认一下 Legacy Boot 为 UEFI，然后插入u盘，将u盘的BOOT优先级提到最高，保存设置并启动。这时候电脑启动时就会进入选择的界面，我们直接选择 ubuntu，这样就进入系统安装的界面了。

系统安装过程中，没有什么要特别注意的。更新和其他软件那里我选择的是正常安装；安装类型里，网上有教程称需要选择其他选项，在这里我选择了与 Windows Boot Manager 共存，目前并未出现太大问题。等待一阵子，系统应当就准备就绪了。

## 对于系统的配置

### 替换国内源

在 **Software & Updates** 中选择国内的镜像，我选择的是阿里云(mirrors.aliyun.com)的镜像

选择之后 Ctrl+Alt+T 打开终端，输入以下指令更新：

```shell
sudo apt update
sudo apt upgrade
```

### 中文输入法

```shell
sudo apt install ibus-libpinyin
sudo apt install ibus-clutter
```

然后重启，找到输入法选项，在 Input Sources 中新增 Chinese(Intelligent Pinyin) 就可以了。

### 双系统时间不统一

这个是系统看待硬件时间方式不一造成的，终端输入：

```shell
timedatectl set-local-rtc 1 --adjust-system-clock
```

禁用 Ubuntu 中的 UTC 即可。

### 取消 sudo 密码（有风险）

```shell
sudo cp /etc/sudoers
sudo visudo
```

找到 `%sudo ALL=(ALL:ALL) ALL` 修改为 `%sudo ALL=(ALL:ALL) NOPASSWD:ALL` 然后 Ctrl+s, Ctrl+x, 重启即可。

### 安装与配置 CMake

在 CMake 官网下载源码，本次安装使用的版本是 3.18.3 的版本。下载完成后打开终端：

```shell
tar -xzvf cmake-3.18.3.tar.gz
cd cmake-3.18.3 
./bootstrap
```

出现报错：

```shell
Cannot find a C++ compiler that supports both C++11 and the specified C++ flags.
......
```

通过指令更新 g++：

```shell
sudo apt-get install g++
```

继续报错：

```shell
Cannot find appropriate Makefile processor on this system.
Please specify one using environment variable MAKE.
```

 输入指令：

```shell
sudo apt-get install build-essential
```

`./bootstrap` 运行到最后再次出错：

```shell
Could not find OpenSSL. Install an OpenSSL development package or configure CMake with -DCMAIKE_USE_OPENSSL=OFF to build without OpenSSL.
```

安装 openssl 的编译依赖：

```shell
sudo apt-get install libssl-dev
```

成功运行，输入`make` ，耐心等待，这一步时间较长。完成后，输入

```shell
sudo make install
```

完成，查看版本 `cmake --version` 看到 `cmake version 3.18.3` 安装成功

### 配置 boost

这里选用的版本是 boost 1.69.0 (其实我也不知道该装哪一个版本)

首先官网下载，解包，进入文件夹：

```Shell
./bootstrap.sh
```

没有问题，下一步

```shell
./b2
```

这一步也比较长，完成之后：

```
sudo ./b2 install
```

配置完成。

### 配置 OpenCV

到现在再配置 OpenCV 是考虑到 OpenCV 需要使用 CMake 以及个人感觉配置可能需要一些库的安装。

同样首先到官网下载源码，`unzip` 解压，进入目录：

```shell
mkdir build
cd build
cmake ..
```

如果没有问题，下一步：

```shell
make
make install
```

配置完成

### 配置 git

```shell
sudo apt-get install git
git --version
```

控制台输出 `git version 2.25.1` 安装完成

## 使用过程中的其他经验在之后进行补充

总体来说，由于很早之前有安装的经验，这次的安装并没有碰到太大的问题，还算顺利。

CSDN链接：https://blog.csdn.net/wwh010802/article/details/108857863

