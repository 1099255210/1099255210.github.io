---
title: Ubuntu 换源
date: 2020-09-31 12:39:08
tags:
---

来到阿里云的镜像网站，查找对应系统需要替换的代码

https://developer.aliyun.com/mirror/

修改 `/etc/apt/sources.list` 中的内容，将原本的内容替换为阿里云镜像站上的的代码

然后控制台输入：

```shell
sudo apt update
sudo apt upgrade
```