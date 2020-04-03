---
title: Davinci-二次开发系列05：echarts-gl map3D扩展
date: 2020-04-03 23:00:00
categories: 
 - [Davinci]
 - [可视化工具]
tags:
 - BI
 - Davinci
---

# echarts-gl

​	介绍一下`echarts-gl`，`charts-gl`专门为`echarts`补充了丰富三维可视化组件。这里的gl应该是global的意思，很多地方简写GL，不建议这样写，因为和真实的GL太冲突，更何况最近肖某的事情，和BL有着千丝万缕的关系，所以，就别和GL扯上关系，我们还是写全称`echarts-gl`。

<!--more-->
# echarts-gl map3D

​	这次介绍`echarts-gl map3D`，就是用`echarts-gl`实现的地图的3D模式，也是在系列03的基础上进行的改造，开启3D，切换地图，效果如下：

![echarts-gl](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/05/echarts-gl.gif)

## echarts-gl安装

```
cnpm install echarts-gl --save 
```

## 页面引入

```
import 'echarts/dist/echarts-gl'  
```

特别注意一下：如果集成了百度地图，这个引入不要放入到`app.tsx`里面，会出现一直加载不完的情况，所以最好放到`chart.tsx`

## 控制参数

​	在样式里面，添加控制参数，开启3D

![is3D](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/05/is3D.jpg)

## render调整

​	在render下面的`map.ts`调整图形，主要是series的属性

```
map：'' // 注意与echarts.registerMap第一个参数保持一致
type: 'map3D' // 通过type指定必须是map3D，还有一种方式geo3D，见echarts官方实例
```

完成以上配置，效果就有了

## echarts-gl事件存在问题

​	`echarts-gl`图表事件一直存在问题，算是个坑，截止目前，我统计了一下官方issue关于事件目前一直open的最少四个，echarts-gl目前维护进度也较慢。

​	试了几个版本的`echarts-gl`，发现对于不同的版本：

- 有版本需要加`getZr()`，有的版本不需要
- 有的事件需要加`getZr()`，有的事件不需要
- 有的版本获取不到区域名称，只能获取到坐标，有的版本能
- 有的版本区域域名加了`getZr()`能获取到，有的不加能获取到，有的都能，有的都不能

​	我目前使用的版本`1.1.1`，单击事件不用加`getZr()`，右键事件需要加，但是右键事件获取不到区域名称，如果发现某个版本单击和右键事件都有，并能获取到取到区域名称，烦请留言告知，在此谢过。

​	就拿我们用的单击和右键事件来说：

​	单击事件实现方式：

```
// 方式一
myChart.on('click', function (params) {
   console.log(params);
});
// 方式二
myChart.getZr().on('click', function (params) {
   console.log(params);
});
```

​	右键事件

```
// 方式一
myChart.on('contextmenu', function (params) {
   console.log(params);
});
// 方式二
myChart.getZr().on('contextmenu', function (params) {
   console.log(params);
});
```

# 几个推荐和注意

## echarts-gl地图下钻

​	如果`echarts-gl` 地图下钻，一些类似边框、颜色等属性，需要在下钻事件里面重新调整，否则下钻到新的地图属性不能匹配。


## echarts-gl map3D组合图形

​	如果仅仅`echarts-gl map3D`效果没那么出众，一般都是配合其它图形，在地图上放柱形、雨滴等会更突出3D效果，所以可以进一步扩展。

![demo-3D](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/05/demo-3D.jpg)

## echarts-gl性能

​	这里map3D较简单，性能未发现问题，echarts-gl有很多空间性很强的图形，这种图形渲染加载都会比2D慢，所以性能也是需要考虑的，其它3D图形用的维度指标较多，性能有待测试验证。

## geo3D和map3d区别

geo3D不支持visualMap，显示3D地理，上面可以加柱状图，各省份数据无法展示。

map3d支持visualMap，可以显示区域数据，不能绘制柱状图，但是也有说可以隐藏一个geo3D来实现，所以待验证，大家可以尝试一下。

# 交流学习

刚建的群，学习Metabase、Davinci等开源BI，群号：72569367，感兴趣的可以加一下。
