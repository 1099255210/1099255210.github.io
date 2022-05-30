---
title: 如何利用 webp 进行小程序图片加载速度的优化
date: 2020-08-18 06:56:08
tags:
---

# 如何利用 webp 进行小程序图片加载速度的优化

## 导语

最近很长一段时间没有更新博客，一方面是自己最近参与了小程序的开发，另一方面也是自己略有些怠惰，给自己记个过~那么现在既然回到学校那么还是要分享一些知识的。

前一阵子参与微信小程序开发时遇到了一个小问题，就是图片的加载速度不理想。即使我们对于图像的大小一缩再缩，但后台服务器中200-300KB的图片在小程序里往往需要等待数十秒才能加载完毕（仅在安卓机上测试过，苹果应该优化得好一些），这个图片加载的速度导致我们的小程序体验比较糟糕。我经过搜索发现webp这种图片格式可能可以为我们提供一个可行的替代方案。

## 知识补充

> WebP（发音：weppy）是一种同时提供了有损压缩与无损压缩（可逆压缩）的图片文件格式，派生自影像编码格式VP8，被认为是WebM多媒体格式的姊妹项目，是由Google在购买On2 Technologies后发展出来，以BSD授权条款发布。
> WebP最初在2010年发布，目标是减少文件大小，但达到和JPEG格式相同的图片质量，希望能够减少图片档在网络上的发送时间。根据Google较早的测试，WebP的无损压缩比网络上找到的PNG档少了45％的文件大小，即使这些PNG档在使用pngcrush和PNGOUT处理过，WebP还是可以减少28％的文件大小。
> <p align="right">维基百科</p>

说人话？好。webp就是谷歌发布的一款可以在保证图片质量较为完整的情况下压缩率较高的网络图片格式。

这么一看，webp岂不是无敌？实际上对于我们的项目它目前还是有局限性的：iOS端的微信小程序是不支持webp的。

但是方法总是有的，既然iOS有它自己的优化方法，那么我就不让它搭上webp这班车了，我们可以判断设备的类型，然后针对安卓的手机进行优化。

## 教程

### 作案工具 

* vscode
* 微信小程序开发工具

### 手段

首先我们需要定义一个全局变量作为判断是否为苹果设备的依据。所有的全局变量均在app.js里定义，在app.js中加入全局变量iOS，并用wx.getSystemInfo获取设备信息进行判断：

```javascript
App({
  onLaunch: function () {
    var that = this;
    wx.getSystemInfo({
      success: (res) => {
        console.log(res);                           //res是一个对象，包含设备信息。其中res.system的内容则是设备系统的版本
        if (res.system.indexOf("iOS") != -1) {      //如果系统版本中有iOS三个字母，则判定为苹果设备
          that.globalData.iOS = true;
        }
      },
    });
  globalData: {
    iOS: false,                                     //默认iOS的值为false
  },
})
```

到这里我们做好了设备类型的判断，接下来我们就要在独立的页面中进行链接替换。打个比方，如果这台设备是苹果，那么图片链接就使用 ../a.png ；如果是安卓，那么链接就换成 ../a.webp 。

给出一个示例页面：

html:

```html
<!--简单的页面，一个view标签，一张图，图片链接用js传入数据-->
<view class="container">
    <image src = '{{img1}}' />
</view>
```

javascript:

```javascript
Page({                                              //传入页面的数据
  data: {
    img1:"../a.png",
  },
})
```

这时我们在js中加入链接替换的内容：

```javascript
const app = getApp();                               //这行代码很重要，用来从app.js中获取全局变量

Page({
  data: {
    img1:"../a.png",
  },
  //事件处理函数，onLoad指在页面加载中的操作
  onLoad: function () {
    if (!app.globalData.iOS) {                      //app.globalData.iOS即为上面定义的全局变量，如果iOS为假，则替换链接
      this.setData({
        img1:"../a.webp",
      });
    }
  },
})
```

这样就完成了webp的替换，还是挺容易的，没有想象中的那么复杂。唯一的问题是页面多了以后替换起来会比较麻烦，不过如果都把图片的数据写在js里的话，替换起来会快很多。
