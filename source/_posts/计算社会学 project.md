---
title: 计算社会学课程 Project
date: 2022-01-19 15:25:08
tags:
---

课堂作业：对班里同学之间的社交关系进行分析

## 使用的软件

> Gephi 是一款网络分析领域的数据可视化软件
>
> 官方网站 - [Gephi](https://gephi.org/)

![image-20220119091559013](https://hgserver-1301261080.cos.ap-seoul.myqcloud.com/snipaste/image-20220119091559013.png)

## 流程

![img](./img/flow.drawio.svg)

## 数据处理与导入

*仅单纯导入了班级内各同学之间认识的关系，并没有将个人其他信息加入*

简单拉出关系表转换为`.csv`

![image-20220119101906964](https://hgserver-1301261080.cos.ap-seoul.myqcloud.com/snipaste/image-20220119101906964.png)

使用 `python` 建立**节点表格**与**边表格**，方便导入 `Gephi` 软件。

![image-20220119102109357](https://hgserver-1301261080.cos.ap-seoul.myqcloud.com/snipaste/image-20220119102109357.png)

## 报告

### 导入时初始网络图

![Snipaste_2022-01-19_10-07-44](https://hgserver-1301261080.cos.ap-seoul.myqcloud.com/snipaste/Snipaste_2022-01-19_10-07-44.png)

*使用的布局方式：Force Atlas，斥力强度 200.0*

### 依据节点的度对节点上色与改变大小

![](./degree.svg)



*使用的布局方式：Force Atlas，斥力强度 2000.0，部分距离较远节点经过手动调整位置。*

### 聚合系数 Cluster coefficient

聚合系数是网络的局部特征，反映了相邻两个人之间朋友圈子的重合度，即该节点的朋友之间也是朋友的程度。

![image-20220119094510712](https://hgserver-1301261080.cos.ap-seoul.myqcloud.com/snipaste/image-20220119094510712.png)

使用`Gephi`计算该网络的**平均聚合系数**为 0.587

### 平均度数 Avg degree

使用`Gephi`计算该网络的**平均度数**为 4.119

度数分布图：

![degree-distribution](https://hgserver-1301261080.cos.ap-seoul.myqcloud.com/snipaste/degree-distribution.png)

### 平均路径长度(特征路径长度) Avg path length

使用`Gephi`计算该网络的平均路径长度为 2.379

网络直径：5

体现**小世界现象**

### 模块化 Modularity

使用`Gephi`计算该网络的**模块化程度**为 0.414

模块化程度：

![](./img/m.jpg)

利用模块化的计算，可以进行社区发现 ( Community Detection )

![image-20220119103903270](https://hgserver-1301261080.cos.ap-seoul.myqcloud.com/snipaste/image-20220119103903270.png)

社区发现可视化结果：

![](./module.svg)

与其它同学发现的对比：

![image-20220119105008234](https://hgserver-1301261080.cos.ap-seoul.myqcloud.com/snipaste/image-20220119105008234.png)

## 附

`python` 源代码

```python
import csv

header_namelist = ['Id', 'Label']
header_relation = ['Source', 'Target']

# generate list
with open('namelist.csv', 'w', newline='', encoding='utf-8') as nf:
    nfer = csv.writer(nf)
    nfer.writerow(header_namelist)
    with open('res.csv', encoding='utf-8') as f:
        f_csv = csv.reader(f)
        next(f_csv, None)
        for row in f_csv:
            nfer.writerow([row[0], row[0]])

# generate relation
with open('relation.csv', 'w', newline='', encoding='utf-8') as nf:
    nfer = csv.writer(nf)
    nfer.writerow(header_relation)
    with open('res.csv', encoding='utf-8') as f:
        f_csv = csv.reader(f)
        next(f_csv, None)
        for row in f_csv:
            source = row[0]
            for target in row[1 : -1]:
                if target:
                    nfer.writerow([source, target])
```

