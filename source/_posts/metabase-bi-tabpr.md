---
title: Metabase-BI系列08：一个关于透视表的PR
date: 2020-02-27 22:40:00
categories: 
 - [Metabase]
 - [可视化工具]
tags:
 - BI
 - Metabase
---

## 概述

​	Excel中经常用到合并单元格，制作复杂表头的表格（透视表）。后台系统、财务系统、报表系统等等对于这种复杂表头的表格还是很刚需的，尤其是像财务这种系统。Excel很擅长这种复杂表头，实现起来很容易，但是软件系统确并那么容易，复杂度越高难度越大，甚至好多系统研究怎么和Excel做集成，一是Excel的强大，二是最终还是要导出Excel的。

<!--more-->
## 透视表和交叉表区别

​	关于透视表和交叉表的区别，你能搜多一些材料，但是不见的能理解。我个人感觉严格的界限也没那么清晰，只要只知道交叉表专指两列（或者多列）分组出现的频率就可以了，更何况交叉表还是一种特殊的透视表呢。概括起来就这么三点

1. 透视表是一种常见的数据汇总工具，进行数据聚合
2. 交叉表是专指分组频率的
3. 交叉表是一种特殊的透视表

## Metabase透视表

​	Metabase小清晰、简洁干练的风格似乎对这种复杂的东西似乎格格不入，尽管Metabase有透视表的功能，但Metabase的透视表真的是简单而且别具一格，基本算不上透视表的功能。正因为不满足，总有人蠢蠢欲动，今天就讲一篇Metabase开源库中很多人热衷的一个PR,，关于Metabase透视表的实现。

​	效果图如下：

![table_summary](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/table/table_summary.png)

## 介绍这个PR

PR：[Metabase pull request #8427](https://github.com/metabase/metabase/pull/8427)

2018年8月30日提出，2020年1月31日由于时间问题和性能问题关闭。

这个复杂表头的表格需求肯定是有的，r-hot进行了尝试，将查询字段按顺序进行分组，并能对分组进行排序，还能显示分组统计。

- 有184次提交记录
- Metabase 源码维护者进行讨论过，查询较多，导致后端沉重，复杂计算存在问题，并提交高级别讨论
- 此复杂表头的表格，支持sql查询方式。
- 对于平均值汇总没有意义，平均值理论应该是先汇总在求值，除非增加新的请求，即使你只要计数和汇总，Metabase聚合是公共组件，就得特殊处理，所以这个确实影响Metabase的标准化
- r-hot用js进行了实现，但是存在js精度不够导致的数据偏差
- r-hot对性能进行了优化
- 有人热衷，有人关注
- open了很长时间
- 最终还是关闭了

这个PR转眼已经过去一年多了，从代码提交来看甚至更早了，所以这个是在v0.33版本（新版本）之前的版本上（大概是v0.30版本）上进行改造的，关于版本历史和v0.33版本可以一查看我的Metabase系列里面文章的介绍。不管怎么说这个pr还是值得关注的，尽管平均值汇总的问题和Metabase标准化的问题不可避免的会存在。

​	这个版本你可以下载下来跑起来看，关于最新版本上的效果，下一篇文章会给出解决方案。这篇重点还是介绍这个pr。既然需求有，或许早晚会出来一版，如果有需求也可以先用r-hot这个版本的实现，一些汇总统计的需求还是满足的。

​	sql查询效果图：

![table_summary03](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/table/table_summary03.png)

## Metabase透视表展示

Metabase透视表是现有的功能，感觉就像交叉表，复杂表头不支持。

- 必须是两个分组列，才默认开启，可以关闭
- 超过两个分组列，不支持透视表

新版透视表如下：

![new01](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/table/new_tsb.jpg)

![new01](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/table/new_tsb01.jpg)

旧版效果图：

![old](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/table/old_tsb.jpg)

![old02](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/table/old_tsb01.jpg)