---
title: gradio 本地部署页面空白的解决方案
date: 2022-09-06 14:53:08
tags:
---

最近在学习如何用简单的 python 代码部署应用，于是接触到了 gradio 这个神器，看着就很好用，而且 huggingface 上的很多模型都在用它作为展示的 demo。

但是在我想在本地部署它的时候，出现了问题：运行之后控制台输出了本地部署的链接，点开之后竟然是一个空白的页面。我的代码并不复杂，内容只有官方文档中的 quick start 的 helloworld 部分：

```python
import gradio as gr

def greet(name):
    return "Hello " + name + "!"

demo = gr.Interface(fn=greet, inputs="text", outputs="text")
demo.launch()
```

（我的本地环境是 windows11，python 3.13，gradio 3.2）

出了这个问题之后我看了一下浏览器控制台的报错信息，显示了一个 js 的引入错误：

```
Failed to load module script: Expected a JavaScript module script but the server responded with a MIME type of "text/plain". Strict MIME type checking is enforced for module scripts per HTML spec.
```

应该就是这个错误导致了无法正确地显示页面，遂以报错信息搜索，找到了一个 [issue](https://github.com/gradio-app/gradio/issues/1203)

这里最终提出了解决的方案：

在 `routes.py` 文件开头加入下面几行代码：

```python
import mimetypes
mimetypes.init()

mimetypes.add_type('application/javascript', '.js')
```



![image-20220906150703488](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220906150703488.png)



同时也了解到这个问题似乎在 windows 上发生得比较多，而且会在未来的版本里修复（如果没有副作用的话，但是这个问题是7.11解决的，现在已经9月了依然没有能在大版本修复），总之还是记录一下吧。