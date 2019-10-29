---
title: 手绘风格的图表库（char.xkcd）
date: 2019-08-20 14:57:11
categories: 
 - 可视化工具
tags:
 - 透明创业实验
 - BI
---
## 	概述

[chart.xkcd](https://github.com/timqian/chart.xkcd) 手绘风格图表库，是[透明创业实验](https://blog.t9t.io/transparent-startup-experiment-2019-05-20/)第十四周发布的产品，关于这个透明创业实验是一个毕业研究生，一个不想让企业拿钱买自己时间的全干工程师，辞职一年搞实验（羡慕），大家有兴趣可以关注一下。

![avatar](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/chart/chart.gif)
<!--more-->


​	首先是关注这个实验，所以了解到这个产品亦或者说是个轮子，其次图表展示这种日常工作中也会用到，主要是关心这个产品实现方式和探究它的价值。

## 	产品价值

仅代表个人观点：

1. 我们用的多的如：百度的echarts、hcharts以及阿里那种可视化工具，功能强大、上手容易，这种手绘风格应该没有，所以差异化和特点具有吸引力

2. 企业和厂家这种风格不太适合，对于一般的尤其是个人网站，用这种风格还是比较有新意

3. 创意的突破总能令人赏心悦目，或许能激发更多的灵感

## 浅析原理

   

- [xkcd]: https://xkcd.com/	"风格"

- D3.js

- 滤镜filter

### xkcd风格

xkcd是兰道尔·门罗（Randall Munroe）的网名，又是他所创作的漫画的名称。作者兰道尔·门罗（Randall Munroe）给作品的定义是一部“关于浪漫、讽刺、数学和语言的网络漫画”(A webcomic of romance,sarcasm, math, and language)，被网友誉为深度宅向网络漫画。

### D3.js

D3 的全称是（Data-Driven Documents），顾名思义可以知道是一个**被数据驱动的文档**。听名字有点抽象，说简单一点，其实就是一个 JavaScript 的函数库，使用它主要是用来做数据可视化的

### 滤镜filter

SVG使用`<filter>`元素来定义滤镜

```
<filter
filterUnits="units to define filter effect region"
primitiveUnits="units to define primitive filter subregion"
x="x-axis co-ordinate" 
y="y-axis co-ordinate"     
width="length"
height="length"
filterRes="numbers for filter region"
xlink:href="reference to another filter" >
</filter>
```

​添加属性

```
<rect x="100" y="100" width="90" height="90" stroke="green" stroke-width="3"
fill="green" filter="url(#filter1)" /> 
```

正式利用这种方式，增加滤镜处理，后续也容易实现自定义和切换风格

简单介绍，后续会持续关注和更新。。。
