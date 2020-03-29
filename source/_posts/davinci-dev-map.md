---
title: Davinci-二次开发系列03：区域地图下钻与选择
date: 2020-03-28 12:00:00
categories: 
 - [Davinci]
 - [可视化工具]
tags:
 - BI
 - Davinci
---

# 概述

系列02中讲到百度地图扩展，地图应用场景多样，对于BI数据的呈现区域地图（非经纬度坐标）似乎应用更加广泛，这次说一下对于区域地图的几点改造。一个是下钻，一个是指定自定义的区域地图。

<!--more-->
## 地图下钻

层级下钻，没有具体的几级，可以一直下钻并返回，这里做了国家、省份、市、区县、街道的下钻。

![drill](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/03/drill.gif)

## 指定地图

指定需要展示的地图，并可以钻取和上卷。

![switch](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/03/switch.gif)

![s&d](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/03/switch%26drill.gif)



# 吐槽一下Davinci地图

Davinci 地图目前十分的不友好，不得不吐槽一下：

- 不能很好的扩展地图

- 地图数据逻辑，通过地理类型和对照js文件去汇总，这种十分的不灵活

- 地图真实应用需求类似下钻不能满足

# 地图改造点

## 去掉原有的地图类型

​	原有地图类型：地图、气泡图、热力图、飞行图，

​	改造后：去掉地图类型

​	这些需求在区域地图上是有的，官方就是考虑这些类型的实现导致区域地图显得复杂，尤其是飞行图这种实际BI应用场景中较少，而且飞行图是要有起点的，默认指定的第一个为起始点，非配置化，略显鸡肋。我们已经在百度地图扩展里做了一些实现，这里直接去掉原有的地图类型，简化代码，更突出区域地图的功能。

![style](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/03/style.jpg)

## 数据类型扩展

​	原类型：地理国家、地理省份、地理城市

​	扩展后：增加地理区县和地理街道

​	备注：实际改造了数据组装逻辑之后，类型仅仅是体现特殊性的地理维度，并不是严格的层级区分了，就是说改造之后实际指定的类型不影响数据展示。

![dimtype](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/03/dimtype.jpg)

## 数据组装逻辑改造

​	原逻辑：根据配置的地理类型，根据层级对照的js文件进行汇总计算。

​	改造后：根据实际数据进行自动汇总处理，与类型无关，即相同字段名称的汇总。

![datastructure](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/03/datastructure.jpg)

## 增加单击下钻右键返回事件

​	添加单击和右键事件实现下钻返回。

```
instance.off('click')
instance.on('click', (params) => {
	// 添加单击事件
})

instance.on('contextmenu', (params) => {
	// 添加右键事件
})
```



## 地图json文件扩展

​	既然能够指定区域地图，就要有地图的json文件，这里推荐两个免费的json地图地址。

​	阿里的：http://datav.aliyun.com/tools/atlas/#&lat=33.54139466898275&lng=104.2822265625&zoom=4

​	个人的：https://gallery.echartsjs.com/editor.html?c=xr1IEt3r4Q  更新很及时，行政区划调整比较及时。

​	中国闭源软件多，开源软件少，可能一个原因是付费的少，这些作者提供的json文件着实给力，所以建议大家使用之余，能力范围内打赏一下（非广告）。

![json](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/03/json.jpg)


# 几个推荐和注意

## json文件改造自定义上传

​	可以改造json文件自定义上传，这样用户可以自己上传json文件然后从地图样式里面指定。

## json层级建立对照字典表

​	对于自定义上传的json文件建立层级对照表，源码里面通过js文件对照的，不利于维护。


## 普通图表下钻功能的影响

​	Davinci dashboard有下钻的功能，并且图表的钻添加了右键事件，所以需要进一步验证是否存在冲突的问题。

# 交流学习

刚建的群，学习Metabase、Davinci等开源BI，群号：72569367，感兴趣的可以加一下。

