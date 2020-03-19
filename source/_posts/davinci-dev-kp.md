---
title: Davinci-二次开发系列01：开篇
date: 2020-03-19 12:00:00
categories: 
 - [Davinci]
 - [可视化工具]
tags:
 - BI
 - Davinci
---

# 概述

Davinci 是宜信出品的DVaaS（数据可视化及服务）的一款BI产品，翻译过来是达芬奇，达芬奇大家都知道著名画家，代表作有《蒙娜丽莎》。所以通过名字就能看出来宜信对着产品的厚望，颇有一种想要成为大师的意思。

百度搜Davinci会搜出DaVinci Resolve这款`不知道你知不知道`的调色软件，所以看来宜信这名字没起好，导致宜信Davinci目前来看已经藏着了百度第二页。

宜信公司：成立于2006年，专注围绕普惠金融和财富管理。

宜信技术学院：宜信技术团队，2017年，宜信技术学院正式成立，通过分享在金融科技领域的开源成果、研发实践促进金融科技生态圈企业创新升级，确实很牛的样子。
<!--more-->

# 宜信开源

主要的开源产品可以在宜信技术学院官网上找到：http://college.creditease.cn/techOpenSource

- [DBus](https://github.com/BriData)：专注于数据的收集及实时数据流计算
- [Moonbox](https://github.com/edp963/moonbox)：是一个DAAS（Data Virtualization as a Service）平台解决方案
- [Wormhole](https://github.com/edp963/wormhole)：是一个SPAAS（Stream Processing as a Service）平台解决方案
- [Davinci](https://github.com/edp963/davinci)：是一个DVAAS（Data Visualization as a Service）平台解决方案

宜信技术学院号称利用开源来支撑智能化运维

# 技术框架

Davinci 采用前端分离的方式，后端java系，前端react，图表echarts。

**前端**：Antd + dva + ES6 + TypeScript + WebPack

- [Antd](https://ant.design/index-cn)：Ant Design of React，基于 Ant Design 设计体系的 React UI 组件库，是蚂蚁金服一套开箱即用的高质量 React 组件，Davici 用Reac 实现，此外Ant Design 还有 Angular、Vue 的实现
- [dva](https://dvajs.com/)：基于React和redux的轻量级elm风格框架，这个很重要，前端修改就必须清楚dva的数据流向，如果有机会，可以单独介绍
- ES6：新一代js语法，过！
- TypeScript：一种由微软开发的[开源](https://baike.baidu.com/item/开源/246339)、跨平台的编程语言，[JavaScript](https://baike.baidu.com/item/JavaScript)的超集，最终会被编译为JavaScript代码
- WebPack：打包工具，过！

**后端**：springboot + mybatis-plus + maven

后端java，标准的springboot、mybatis-plus框架，容易理解。

此外采用swagger框架生成api接口说明。

**图表库**：[echart](https://www.echartsjs.com/zh/index.html)，特点就是使用和集成简单，但是没有D3那么灵活。



# 二次开发

Davinci 源代码github地址：https://github.com/edp963/davinci

二次开发完全可以参考Davinci 的使用手册的[文档说明](https://edp963.github.io/davinci/docs/zh/1.1-deployment)，介绍比较详细，这里只说几个注意点。

## 注意点一：分支

这里需要注意一下，目前Davinci默认的分支是dev-0.3，目前Davinci最新维护的版本是0.3版本，所以clone最新dev-0.3版本即可。

## 注意点二：前端install

Davinci 前端启动install的时候，建议使用`cnpm install`，不要用`npm install`即使设置使用淘宝源也容易出现问题。

## 注意点三：前端启动错误

前端启动会发现有很多文件报错，有些可能是严格校验导致的，大部分确实存在严格意义上的错误，但是不用管这些错误就可以了，不影响使用和二次开发。

## 注意点四：前端发布

Davinci 前端发布访问的是davinci-ui文件夹下面，前端打包在webapp的build下面，所以如果正式发布，需要将build下的文件拷贝到davinci-ui下面，bin下面未提供自动前端发布的脚本命令，此外由于每次打包生成的前端文件命名不一样，davinci-ui下的原文件删除即可。

## 注意点五：邮件定时任务

除了邮件的邮箱配置正确以外，如果想正常接收，需要安装[phantomjs](https://phantomjs.org/)。

# 点评一下

Davinci概览里面已经把架构、设计理念、功能特点等等介绍的挺全面的了，这里在突出强调几个点：

- 数据行列权限控制的比较全
- 大屏display功能亮点，大屏数据可视化展示已然成为未来的趋势，目前不完善，提升改造空间大
- 功能确实很全，透视表功能也比较完善

针对二次开发的优缺点：

- java系并采用主流框架易于二次开发
- 架构清晰，易于理解
- 开源时间短，社区论坛没有，资料相对较少
- 功能多，bug也多
- 代码质量略差，有明显多人开发和功能需求增加引起的规范性上的不完美
- 前端界面尤其移动端效果较差，这个同样也给二次开发带来更大的提升空间

不管怎么说，Davinci还是挺不错的一块BI产品，也比较适合二次开发。

# 交流学习

刚建的群，学习Metabase、Davinci等开源BI，群号：72569367，感兴趣的可以加一下。

