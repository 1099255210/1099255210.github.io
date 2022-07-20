---
title: Hugging Face 学习(1)
date: 2022-07-12 12:00:06
tags:
---

# Hugging Face 学习(1)

Hugging Face是一个非常流行的 NLP 库，Hugging Face 的 [官网](https://huggingface.co/) 上也有很多开源的模型。

官网上的有些模型已经提供了简单的使用方法。比如我最先接触到的这个 `dalle-mini` 模型，提供了一个生成的接口(虽然好像还是定向到了[其它网站](https://www.craiyon.com)，据说是因为访问量太大了)，可以输入一段文本，生成一些图片。这里随便输入一个句子 A Rubic's Cube swimming in the pool (水池里游泳的魔方)：

![image-20220720084135775](https://cdn.jsdelivr.net/gh/1099255210/blogimgrepo@main/img/image-20220720084135775.png)



关于 `dalle` 这个模型，最近看到一篇文章比较有意思(https://blog.csdn.net/fengdu78/article/details/125109103)

---

如果是在本地使用 Hugging Face 的一些 NLP 模型，可以安装他们提供的 `transformers` 的库。

```shell
pip install transformers
```

这里使用一个 [t5-small](https://huggingface.co/t5-small) 的模型测试一下。

t5 (Text-To-Text Transfer Transformer) 是一个统一框架，将所有 NLP 任务都转化成 Text-to-Text （文本到文本）任务。

![](https://1.bp.blogspot.com/-o4oiOExxq1s/Xk26XPC3haI/AAAAAAAAFU8/NBlvOWB84L0PTYy9TzZBaLf6fwPGJTR0QCLcBGAsYHQ/s640/image3.gif)

```python
from transformers import AutoTokenizer, AutoModelWithLMHead

tokenizer = AutoTokenizer.from_pretrained("t5-small")

model = AutoModelWithLMHead.from_pretrained("t5-small")
```

只需要三行代码，就可以直接加载模型开始使用：

```python
input_ids = tokenizer("translate English to Germany: What a nice day.", return_tensors="pt").input_ids

outputs = model.generate(input_ids)

print(tokenizer.decode(outputs[0], skip_special_tokens=True))
```

得到输出：

```
Was für ein nettes Tag.
```

