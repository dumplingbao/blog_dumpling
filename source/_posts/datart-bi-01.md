---
title: datart系列01：图表插件开发作品大赛
date: 2022-03-27 10:00:00
categories: 
 - [Datart]
 - [可视化工具]
tags:
 - BI
 - Datart
---

## 概述

2022年的第一篇，最近疫情+航空事故，虽是春暖花开，还是宅在家开码。借着datart图表插件开发作品大赛，给开了个头，datart的头，写作的头，但愿后续多写，今年估计的写的方向也会比较分散。

这一篇就当是参加图表插件开发作品大赛的一次开篇总结，也算是预热，这次采用datart自定义插件的形式，全部用自定义插件的方式，不改动源代码，不管怎么说，datart自定义插件发现越用越丝滑，很看好这一特性。后续再写datart源码及二次开发。

<!--more-->

## 作品：

### 作品1：海洋鱼馆（动画）

这个作品算是魔改，但确实有着特殊的应用场景

这个在Davinci的时候做过扩展，这次全新的素材

造了鱼馆的轮子来适配datart的插件，后期我们再展开思路讲实现，并开源出来（包括素材）

![](https://cdn.disscode.cn/blog/datart/01/yg.jpg)

### 作品2：地图（echarts）

这个也在Davinci扩展了，这次也是做了集成，场景没什么好说的，直接看效果，这个本来也想套一层封装，发现有点问题，直接用原生js做的集成

![](https://cdn.disscode.cn/blog/datart/01/map.jpg)

### 作品3：智能仓库（threejs）

智能数字化车间，3D车间模型等等，这种3D场景化很多人都追求，甚至是偏执。查了资料，看了Threejs官网所有的demo，逛了社区，确实没找到高大上且合适的场景化模型。就从网上找了个智能仓库的场景做了集成，个人理解这种3D场景就是先做场景化的模型（这种模型确实需要专业人来做，上手有门槛），在场景位置上展示数据或者图表。这个也是造了适配datart的轮子，算是个demo，半成品吧，后期展开讲，也开源出来，有专业水平的可以做模型然后集成到datart。

![](https://cdn.disscode.cn/blog/datart/01/threejs.jpg)



### 作品4：手绘风格（D3）

这个效果没做成功，在Davinci的时候因为受限于echarts就没有做，这次可以扩展D3，本以为这个应该是最简单，确没有成功，初步判断应该是iframe嵌套导致滤镜不成功，没找到合适的方案。不过除了滤镜D3其它的效果集成还是没有任何问题的，后续再研究。

![](https://cdn.disscode.cn/blog/datart/01/xkcd.jpg)

## 总结：

总结一下datart自定义插件，对于前端来说可能算不上新技术，但是对于BI来说就是很好一次微创新：

- 特殊化定制，满足个性化需求
- 上手容易，官方这块文档很详细
- 扩展灵活，很丝滑
- 像D3这种灵活性很高的，本是就具备无限可能，所以datart也具备无限可能