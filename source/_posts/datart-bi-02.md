---
title:  datart系列02：图表插件开发作品
date: 2022-04-05 11:00:00
categories: 
 - [Datart]
 - [可视化工具]
tags:
 - BI
 - Datart
---

## 概述

今天整理了两个为未能参赛的作品，个人感觉还是比较好的两个点，就索性整理发出来。

<!--more-->

## 作品：手绘风格（D3）

这个上一篇中说到了，也是时间的问题，未能找到原因，本以为是iframe嵌套导致的，后来官方给了去掉iframe的配置的方法进行了尝试，发现不是这个原因。

其实到现在也没有完全定位不能正常的渲染的原因，今天就换了种实现方式，放弃直接用开源的轮子，直接写源代码到插件里面，发现能够正常展示了，后续在把其它图表的补充完整。效果如下：

![xkcd01](https://cdn.disscode.cn/blog/datart/02/xkcd01.jpg)

![xkcd01](https://cdn.disscode.cn/blog/datart/02/xkcd02.jpg)

![xkcd01](https://cdn.disscode.cn/blog/datart/02/xkcd03.jpg)

## 作品：3D地图（CesiumJs）

关于Threejs、WebGL、CesiumJs这里不做赘述，虽然CesiumJs受众相对较小，而且偏重GIS，但是利用cesiumjs做数字孪生应该也有很多了，所以就集成到datar插件试一下，而且CesiumJs号称永久开源免费，但是这个完全不在参赛作品计划内，原因很简单，用到了付费插件，虽然费用很低，做项目没有任何问题，但是参赛开源就不好了。但是CesiumJs作为datart插件集成还是很简单的，单纯的3D地球，肯定满足不了实际场景，但是花点实际研究研究API搞点特效出来，还是不难的，而且CesiumJs好用，效果出众，下面就看一下效果：

![xkcd01](https://cdn.disscode.cn/blog/datart/02/cesium01.jpg)

![xkcd01](https://cdn.disscode.cn/blog/datart/02/cesium04.jpg)
