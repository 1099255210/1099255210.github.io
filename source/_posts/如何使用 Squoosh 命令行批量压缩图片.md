---
title: 如何使用 Squoosh 命令行批量压缩图片
date: 2022-09-22 23:09:08
tags:
mathjax: false
---

先说明：windows 上无法批量压缩，原因：

https://github.com/GoogleChromeLabs/squoosh/issues/973

因为 squoosh-cli 暂不支持 windows 上的通配符。这么一个小问题，给我整崩溃了。

完整过程如下

先在 (https://squoosh.app) 上测试一下图片压缩的设置：

![image-20220922011515991](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220922011515991.png)

一切就绪后，点击右侧菜单 `Edit` 右边的控制台小图标，将命令复制下来，应该类似于这样：

```shell
npx @squoosh/cli --resize '{"enabled":true,"width":970,"height":970,"method":"lanczos3","fitMethod":"stretch","premultiply":true,"linearRGB":true}' --mozjpeg '{"quality":75,"baseline":false,"arithmetic":false,"progressive":true,"optimize_coding":true,"smoothing":0,"color_space":3,"quant_table":3,"trellis_multipass":false,"trellis_opt_zero":false,"trellis_opt_table":false,"trellis_loops":1,"auto_subsample":true,"chroma_subsample":2,"separate_chroma_quality":false,"chroma_quality":75}'
```

如果想要先安装再使用，可以先用以下命令：

```shell
npm i -g @squoosh/cli
```

然后就可以用 `squoosh-cli` 命令了，将上面的命令改写一下：

```shell
squoosh-cli --resize '{"enabled":true,"width":970,"height":970,"method":"lanczos3","fitMethod":"stretch","premultiply":true,"linearRGB":true}' --mozjpeg '{"quality":75,"baseline":false,"arithmetic":false,"progressive":true,"optimize_coding":true,"smoothing":0,"color_space":3,"quant_table":3,"trellis_multipass":false,"trellis_opt_zero":false,"trellis_opt_table":false,"trellis_loops":1,"auto_subsample":true,"chroma_subsample":2,"separate_chroma_quality":false,"chroma_quality":75}'
```

还有需要改动的地方就是：目标文件夹，目标文件，长宽

```shell
squoosh-cli --resize '{"enabled":true,"width":970,"height":970,"method":"lanczos3","fitMethod":"stretch","premultiply":true,"linearRGB":true}' --mozjpeg '{"quality":75,"baseline":false,"arithmetic":false,"progressive":true,"optimize_coding":true,"smoothing":0,"color_space":3,"quant_table":3,"trellis_multipass":false,"trellis_opt_zero":false,"trellis_opt_table":false,"trellis_loops":1,"auto_subsample":true,"chroma_subsample":2,"separate_chroma_quality":false,"chroma_quality":75}' -d "[destinationfolder]" "*.png"
```

在 `-d` 参数后写上输出的文件夹，后面再跟需要压缩的图片，可以使用通配符（windows不行！），然后前面 `--resize` 的参数里长宽可以只保留一个参数，就不会压缩图片长宽比了：

```shell
squoosh-cli --resize '{"enabled":true,"width":500,"method":"lanczos3","fitMethod":"stretch","premultiply":true,"linearRGB":true}' --mozjpeg '{"quality":75,"baseline":false,"arithmetic":false,"progressive":true,"optimize_coding":true,"smoothing":0,"color_space":3,"quant_table":3,"trellis_multipass":false,"trellis_opt_zero":false,"trellis_opt_table":false,"trellis_loops":1,"auto_subsample":true,"chroma_subsample":2,"separate_chroma_quality":false,"chroma_quality":75}' -d "[destinationfolder]" "*.png"
```

然后就可以舒适地观看转换过程：

![image-20220922012116526](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220922012116526.png)