---
title: Clip 模型与其应用
date: 2022-09-04 20:09:08
tags:
mathjax: false
---

# Clip 模型与其应用

## CLIP 模型的介绍

CLIP 模型使用的方法：对比学习，预测 n*n 对图像与文本数据，将图片分类任务转换成图文匹配任务。这个过程实际上就是引入了 NLP 给出的监督信号。

![image-20220904193225832](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220904193225832.png)



图中左侧，得到文本特征与图片的特征后可以看到，对角线上的元素都是图文匹配的，共有 N 个正样本，其余的元素都是负样本，共有 N*N-N 个。其中，文本数据使用Transformer，图片数据用了两种模型，ResNet 和 Vision Transformer (ViT)。

CLIP 模型训练所用的数据集较为庞大，包含从互联网上各种公开资源收集的4亿对图像、文本，CLIP是从头开始训练的，没有使用预训练的初始参数。

## CLIP 的应用

## 利用 CLIP 做简单的图片检索

在 jupyter notebook 中做一个简单的问答系统：

```python
import torch
import clip
import pandas as pd
import numpy as np
import os
from PIL import Image
from IPython.display import display
from tqdm.notebook import tqdm

device = "cuda" if torch.cuda.is_available() else "cpu"
model, preprocess = clip.load("ViT-B/32", device=device)

# load images

data_location =  "./imgs"
img_dict = {}
for inx, f in enumerate(os.listdir(data_location)):
  img_dict[inx] = f
img_nums = len(img_dict)
print("There are {} images.".format(img_nums))

if img_nums == 0:
  print('no image in the folder.')
```

这一段用来加载模型 以及 读取文件夹中所有的图片

```python
# get text input from user && text encode
instr = input('Pleaze input description:')
text_input = clip.tokenize(instr).to(device)
with torch.no_grad():
  text_f = model.encode_text(text_input)

sim = {}
pbar = tqdm(total=img_nums)

for i in range(img_nums):
  
  image_path = f'{data_location}/{img_dict[i]}'
  img = Image.open(image_path)
  img_input = preprocess(img).unsqueeze(0).to(device)

  # image encode
  with torch.no_grad():
    img_f = model.encode_image(img_input)

  # calculate similarity
  img_f /= img_f.norm(dim=-1, keepdim=True)
  text_f /= text_f.norm(dim=-1, keepdim=True)
  similarity = 100 * img_f @ text_f.T
  sim[i] = similarity
  pbar.update(1)

# display top3 result
res = sorted(sim.items(), key=lambda s:s[1], reverse=True)
print(f'query:{instr}')
MAX_SIZE = (300, 300)
for i in range(3):
  image_path = f'{data_location}/{img_dict[res[i][0]]}'
  img = Image.open(image_path)
  img.thumbnail(MAX_SIZE)
  display(img)
```

这里先读入用户的输入，即搜索图片的关键词/句，然后将文本编码得到特征，然后分别对应所有图片的特征计算相似度，取相似度最高的三张图片输出。

图片我没有找网上的一些数据集（因为大部分应该已经有人测试过了），我想看一看这个模型到底有多强大，于是我选取了26张我自己拍的一些生活照，进行了一下图像压缩，控制在500kb之内，作为本实验的数据集。

经过我的一些尝试，发现准确度还是很高的，得到的结果令人满意。下面展示一些查询的结果：


![image-20220904195212924](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220904195212924.png)

![image-20220904195756669](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220904195756669.png)

![image-20220904200643952](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220904200643952.png)



值得一提的是，一开始我并没有预料到他能学习到图像中的文本特征，但是实验中我发现，如果图片中有文字的话，也能被它检索到，比如这个关键词：


![image-20220904200822620](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220904200822620.png)

![image-20220904200950871](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220904200950871.png)


我这里是输入了这个midi键盘的名称（是刻在键盘上的文字），结果它也能给我识别出来，感觉非常的神奇。

