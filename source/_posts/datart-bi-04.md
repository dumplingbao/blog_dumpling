---
title: datart系列04：基于threejs自定义插件3D-MAP
date: 2023-01-11 13:01:08
categories: 
 - [Datart]
 - [可视化工具]
tags:
 - BI
 - Datart
---

## 概述

datart自定义插件的方式，基于threejs自定义插件3D-MAP。

## 应用场景

根据经纬度进行的城市数据展示场景，适用数据大屏，3D地图等
<!--more-->
## 验证数据

见插件文件：`air_quality.sql`、`air_quality_sd.sql`

## 前置条件

1. 三个纬度、一个指标
2. 三个纬度有顺序要求（1、需要显示的名称，2、经度，3、纬度）

备注：三个维度有顺序要求是为了保证不改动源码扩展插件，动源码有两种解决方式如下：

第一种：增加经度、纬度的维度类型，根据类型判断

第二种：扩展经度、纬度的维度拖入框，类似双Y轴方式

## 规则设定

- **地图：**地图json文件url地址
- **基准值：**适配不同数据级别展示，推荐设置样本数据最大值

- **缩放级别：**地图缩放等级，适配不同区域展示情况
- **效果：**水波、气泡、热力图
- **中心坐标：**设置中心坐标的经度、纬度，控制地图显示区域

## 使用说明

插件地址：https://github.com/dumplingbao/datar-plug

```
		<div id="container"></div>
    <script>
      let f1 = new dumplingbaoThree.Map(
        document.getElementById('container'),
        {
          data: {
            // 城市数据
            cityData:
              [
                {
                    city: "青岛",
                    value: 100,
                    EN: [120.3844, 36.1052],//经纬度
                },
                {
                    city: "济南",
                    value: 89,
                    EN: [117.00, 36.40],
                }
            ]
          }
        },
        {
          // config: {
          //   mapJson: 'https://geo.datav.aliyun.com/areas_v3/bound/370000_full.json',
          //   valueMax: 100,
          //   level: 3,
          //   longitude: 118.77,
          //   latitude: 36.40
          // },
          config: {
            // 地图json地址
            mapJson: 'https://geo.datav.aliyun.com/areas_v3/bound/100000_full.json',
            // 基准值，推进样本数据最大值
            valueMax: 100,
            // 缩放级别
            level: 16,
            // 中心坐标经度
            longitude: 105.11,
             // 中心坐标纬度
            latitude: 35.98
          },
        }
      );
    </script>
```

## 备注：

1. 根据datart插件使用说明进行操作
2. 根据需要优化后提交官方自定义插件库，部分规则依据当前使用的版本，命名规则等未按官方提供插件库的方式
3. 地图json文件推荐，阿里云datav：http://datav.aliyun.com/portal/school/atlas/area_selector
4. 私有化部署，只需把文件放到私有化文件服务器或者可访问的地址即可
5. 省份地图，推荐缩放级别在3或4即可，省份中心坐标自行百度
6. 插件存在已知bug，后续修复，正常使用满足

## 效果展示

![3d-01](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/datart/04/3d-01.png)

![3d-02](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/datart/04/3d-02.png)

![3d-sd](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/datart/04/3d-sd.png)