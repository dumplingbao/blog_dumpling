---
title: Davinci-二次开发系列02：widget百度地图扩展
date: 2020-03-19 12:00:00
categories: 
 - [Davinci]
 - [可视化工具]
tags:
 - BI
 - Davinci
---

# 概述

Davinci 目前的地图是区域地图，取自json文件，地图目前是写死状态，区域地图下还有气泡图、飞行图和热力图，存在bug且这种写死的状态实在很难满足真实需求。

Davinci 目前没有集成百度或者高德地图，下面就说一下扩展百度地图的思路，其实也适用于echarts下的图表扩展，只不过地图有点特殊，由于echarts官网有百度地图的demo，所以选择百度地图，扩展起来应该更容易，高德地图也是可以的。
<!--more-->

## widget介绍

Davinci 设计理念里面指出，Davinci **围绕 View（数据视图）与 Widget（可视化组件）两个核心概念设计**，其中Widget就是我们说的饼图、柱状图、表格、折线图等等组件，其实对于BI软件来说，数据的图表呈现是很重要的一部分，而且Widget工作台也是Davinci里面最复杂的一块。

Davinci workbench工作台如图：

![widget02](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/01/widget02.png)

# workbench

Davinci workbench工作台是很核心的且复杂的一部分，对于echart图表的扩展，主要对其修改即可。

## 透视驱动和图表驱动

透视驱动：以[透视表](https://en.wikipedia.org/wiki/Pivot_table)为基础的可视化展示逻辑，可以简单认为复杂度比较高的，但是也强调了*目前透视驱动的配置项完善度不如图表驱动*，关于透视表可以看一下之前写的关于透视表PR那篇blog。

图表驱动：图表驱动即为常规的、基于图表分类的可视化展示逻辑。

## workbench前端代码目录

```
└── webapp
  └── app
    └── containers
      └── Widget                  //widget功能所在文件夹
        ├── component                 //组件
        | ├── Chart（图表驱动）
        | ├── Config
        | ├── Pivot（透视驱动）
        | ├── Widget
        | └── Workbench                   //工作台显示区（图表、数据、样式、配置）
        | | ├── ConfigSections                //各个图表样式（图例、颜色、标签、xy轴等）配置
        | | | ├── SpecSection
        | | | └── TableSection
        | | └── Dropbox
        ├── config                    //初始化配置信息（数据、样式、配置）
        | ├── chart（图表驱动）
        | └── pivot（透视驱动）
        ├── render					  //根据信息最后处理（拼装echarts需要的配置）
        | ├── chart（图表驱动）
        | └── pivot（透视驱动）
        ...
        
```

## echarts组件扩展

概括起来就三个步骤

1. 扩展图表驱动图标

   ![widget-icon](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/01/widget-icon.jpg)

2. 扩展图表样式（也可以用原有的，一般每个图表都有自己特有的一些样式配置）

   可以参见`webapp/app/containers/Widget/component/Workbench/ConfigSections/SpecSection/specs`下的任意一个

   ![widget-style](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/01/widget-style.jpg)

3. 扩展图表echarts拼装

   可以参见`webapp/app/containers/Widget/render/chart`下的任意一个

# 百度地图扩展效果

## 气泡根据数据变化大小和颜色

![widget-bubble](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/01/simple-bubble.jpg)

## 气泡根据数据变化颜色，大小不变

![simple=bubble](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/01/simple%3Dbubble.jpg)

## 气泡水波效果

![simple-wave](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/01/simple-wave.jpg)

## 绿色雨滴效果

![green-rain](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/01/green-rain.jpg)

## 黑色热力效果

![night-heat](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/01/night-heat.jpg)

# 几个推荐和注意

## 百度地图秘钥

需要去百度地图开放平台申请秘钥，并引入百度地图插件

放到app下面index.html里即可

```
<script src="http://api.map.baidu.com/api?v=2.0&ak=秘钥"></script>
```

## react引入echarts百度地图扩展

可以放到app.tsx里面，也可以放到widget组件chart.tsx里面（推荐）

```
import 'echarts/extension/bmap/bmap'
```

## 图标使用阿里的iconfont

Davinci 使用的就是阿里的iconfont，所以这了也建议使用，存放在assets/font下面，像ttf和woff文件需要找专门的工具做导入，比较多容易搜到。

## echarts地图和其它图表切换问题

比如饼图切换柱状图，直接clear就行，但是百度地图不能直接clear所以需要dispose以下然后再创建，目前没有找到更好的办法。

## 数据和echarts组装参见官网

空气质量-百度地图地址：https://www.echartsjs.com/examples/zh/editor.html?c=effectScatter-bmap

## 地图主题使用地图编辑器

官方编辑器，个性化地图：http://lbsyun.baidu.com/apiconsole/custommap

如果想要现成的推荐使用，百度地图个性在线编辑器：https://developer.baidu.com/map/custom/

可以自动生成配置的json文件，强烈推荐！！！

## 中心坐标和缩放比例

目前中心坐标和缩放比例手动输入，不能从百度地图自动获取

## 气泡大小随数值大小变化问题

数据量级跨度较大的数据，不能很好的展示气泡大小显示效果，可以用最大最小值做一些简单的处理，期待有更好的算法规则。

# 交流学习

刚建的群，学习Metabase、Davinci等开源BI，群号：72569367，感兴趣的可以加一下。