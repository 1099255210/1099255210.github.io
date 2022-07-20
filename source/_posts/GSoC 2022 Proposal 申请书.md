---
title: GSoC 2022 Proposal 申请书
date: 2022-04-08 21:17:08
tags:
---

今年终于鼓起勇气参加了 GSoC (Google Summer of Code)，加入的组织是去年我就看上的 GNU Octave。

今年的 GSoC 我还是做了一些准备工作，写下了一些官方相关文档的解释，感兴趣的可以查看：

[Time management for contributors 开发时间安排](https://hackmd.io/@1099255210/S117Bf6Qq)

[GSoC 2022 Timeline 时间线](https://hackmd.io/@1099255210/S1SUtEKQc)

以下是我的 GSoC 2022 Proposal 申请书。

（2022/5/21 注：这篇申请书最终没有通过，我的 GSoC 2022 计划最终只能搁浅。具体原因可能有很多吧，不过我还是挺希望自己之后能最终独立完成这个项目。）

# YAML Support for GNU-Octave

> Name: Han Wang
>
> Email: jmfymxx@gmail.com
>
> Github: https://github.com/1099255210
>
> Accout on [Octave Discourse](https://octave.discourse.group/): wh-1099255210

## Project Description

YAML, as well as JSON, is a very common data format. GNU Octave has built-in support of JSON, but still lacks of YAML support. The goal of the project is to implementing YAML support into Octave with [Rapid YAML](https://github.com/biojppm/rapidyaml) (a fast C++ library). During the project, an Octave package containing `yamldecode()` & `yamlencode()` functions with proper documentation and unit tests will be created, and the package will be considered to be merged into core Octave. 

## Related Work

In GSoC 2020, JSON format has been successfully implemented. The package [pkg-json](https://github.com/gnu-octave/pkg-json) is a good example on how to encode and decode this kind of data format. The pkg-json used [rapidjson](https://github.com/Tencent/rapidjson) (a JSON parser and generator for C++), while in my project I decide to use [Rapid YAML](https://github.com/biojppm/rapidyaml), which is a C++ library to parse and emit YAML.

## My Project Plan

First, I have to get familiar with YAML data format in case to know the proper conversion between YAML data types and Octave data types. I have made a table on this:

From YAML to Octave data (will be used in `yamldecode()`)

| YAML Data Type                              | Octave Data Type            |
| ------------------------------------------- | --------------------------- |
| Number                                      | scalar double               |
| Boolean                                     | scalar logical              |
| String                                      | character vector            |
| Dictionary                                  | scalar struct               |
| List, of Numbers                            | double array                |
| List, of Booleans                           | logical array               |
| List, of Strings                            | string array                |
| List, of Dictionary - Same field names      | struct array                |
| List, of Dictionary - Different field names | cell array of scalar struct |
| null, in numeric lists                      | NaN                         |
| null, in nonnumeric lists                   | Empty double array          |

From Octave data to YAML (will be used in `yamlencode()`)

| Octave Data Type   | YAML Data Type                                        |
| ------------------ | ----------------------------------------------------- |
| scalar logical     | Boolean                                               |
| logical vector     | List, of Booleans, reshaped to row vector             |
| logical array      | List, of Booleans, nested                             |
| scalar numeric     | Number                                                |
| numeric vector     | List, of Numbers, reshaped to row vector              |
| numeric array      | List, of Numbers, nested                              |
| NaN, NA, Inf, -Inf | null (1) \  "NaN", "NaN", "Infinity", "-Infinity" (2) |
| character vector   | String                                                |
| character array    | List, of Strings                                      |
| scalar cell        | List                                                  |
| cell vector        | List, reshaped to row vector                          |
| cell array         | List, flattened to row vector                         |
| scalar struct      | Dictionary                                            |
| struct vector      | List, of Dictionaries, reshaped to row vector         |
| struct array       | List, of Dictionaries, nested                         |

(1) `ConvertInfAndNan = true`

(2) `ConvertInfAndNan = false`

With tables above, it will be much easier to convert data from one side to the other side. I just need to recursively decompose data, convert them respectively to what they should be, then put them together as an output.

## My Timeline

![image-20220408144000601](https://hgserver-1301261080.cos.ap-seoul.myqcloud.com/snipaste/image-20220408144000601.png)

## About Me

I'm currently a junior CS student. I'm good at C/C++ programming, and work with JSON/YAML very often, so I'm familiar with them. I'm used to putting my codes on github and using git to manage them, but I've never really been a contributor in open source projects. So this is my first time to be a part of the open source community, and I'm really excited about it.

During the past few weeks, I have joined the **GNU Octave Developer Community,** and have been a member in **Octave discourse**. Under the guidance of the mentor, I have created the [pkg-yaml](https://github.com/gnu-octave/pkg-yaml) repository on github, committed my code to it. The package can now be installed, and has the function to parse simple YAML data to Octave data. I also sent a pull request to [Rapid YAML](https://github.com/biojppm/rapidyaml) to help fix their amalgamate script to generate header files, and it [successfully got merged into master branch](https://github.com/biojppm/rapidyaml/commit/a04663f55fa1d5a58c873017e24849309a29c226).

I believe with my enthusiasm and patience, I will be up to this job.