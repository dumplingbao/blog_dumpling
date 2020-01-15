---
title: Metabase-BI系列07：V0.33版本焕然一新
date: 2020-01-15 12:00:00
categories: 
 - [Metabase]
 - [可视化工具]
tags:
 - BI
 - Metabase
---
## 概述

2019年4月23日，Metabase官方发布声明，已获得A轮融资，NEA向Metabase投资了800万美元。很重要的一点是NEA具有与Tableau（关于tableau有兴趣可以去官网下载demo）进行孵化和合作的经验，与[MongoDB](https://www.mongodb.com/)，[Elastic](https://www.elastic.co/)，[Nginx](https://www.nginx.com/)和[Databricks](https://databricks.com/)等开源公司有广泛合作经验。

而且Metabase过去的一年中悄悄的发布企业版，虽然在Metabase企业版在论坛中已不是什么秘密，但是Metabase还是强调构建最好的开源产品始终是Metabase的重点。

很快Metabase于2019年6月24发布了v0.33 preview（预览版），8月19日发布v0.33正式版，并在12月19日双旦之前发布v0.34Holiday（假日版），通过v0.33预览版之后，会发现真的是焕然一新。

目前网上流通的Metabase相关的资料绝大部分都是v0.33之前版本，所以在此强调一下，v0.33不仅新而且值得一提，对于Metabase的意义也足够重要，所以将此版本单独写出来。
<!--more-->

## 更新记录表

| **序号** | **版本**      | **版本变化**               | **描述**                                                     |
| -------- | ------------- | ---------------------- | :----------------------------------------------------------- |
| 1.       | V0.33 Preview | 查询生成器             | 1、创建关联表重构  2、分阶段进行数据聚合、分组、过滤、自定义字段等 |
| 2.       | V0.33 Preview | 探索模式               | 1、数据可视化功能重构  2、汇总聚合功能调整                   |
| 3.       | V0.33 Preview | UI更新                 | 1、表格样式重新定义和调整  2、过滤器工作方式调整  3、数据呈现调整等等 |
| 4.       | V0.33 Preview | 删除sql预览功能        | 去除功能，调整查询生成器中                                          |
| 5.       | V0.33         | 查看表和可视化模式切换 | 数据展示界面，增加表格和可视化两种数据切换模式               |
| 6.       | V0.33         | 导航栏浏览数据源       | 快捷入口，减少返回主页等中间操作                             |
| 7.       | V0.33         | 增强搜索               | 包括问题、仪表板以及更细分，搜索功能加强                     |
| 8.       | V0.33         | 翻译政策调整           | 制定新的翻译策略                                             |
| 9.       | V0.34 Holiday | 显示数据点上的值       | 查看条形图时，我们将自动为您显示这些点上方的值               |
| 10.      | V0.34 Holiday | 新增mongo变量          | 允许在本机（SQL）查询中使用变量                              |
| 11.      | V0.34 Holiday | 时区修复               | 1、 数据库驱动时区调整  2、 时间处理时区问题调整             |
| 12.      | V0.34 Holiday | SQL编辑器升级          | 1、 调整编辑器大小  2、 变量面板增加新图层                   |

## 版本变化

旧版本：V0.33预览版之前版本

新版本：V0.33预览版及之后版本

### 旧版UI

![old_ui](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/old_ui.jpg)

### 新版UI

数据、聚合等功能集成到生成器，隐藏后与旧版比较更加简洁，旧版功能集成在上方占据空间，但是各有所爱

![new_ui](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/new_ui.jpg)

### 旧版生成器

生成器是v0.33版本的叫法，旧版没有，我们姑且把数据、筛选、聚合、分组等功能都算在生成器里面

![old_scq](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/old_scq.jpg)

### 新版生成器

新版生成器好像隐藏起来，像帘子上下展开而非弹窗的形式。

![new_scq](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/new_scq.jpg)

### 新版动态图展示

![Metabase01](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/metabase01.gif)

![Metabase02](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/metabase02.gif)

### 旧版图表显示

![old_tubiao](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/old_tubiao.jpg)

### 新版图表显示

![new_tubiao](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/new_tubiao.jpg)

### 旧版设置

弹窗是形式

![old_setting](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/old_setting.jpg)

### 新版设置

去掉原来弹窗模式

![new_setting](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/new_setting.jpg)

### 旧版下载

![old_download](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/old_download.jpg)

### 新版下载

![new_download](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/new_download.jpg)

### 新版数据源入口

增加快速数据入口

![new_data](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/new_data.png)

### 新版搜索

增强搜索功能

![new_search](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/new_search.png)

### 新版图表和数据切换

新版图表和实际数据能够快速切换

![new_table01](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/new_table01.png)

![new_table02](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/new_table02.png)

### 新版优化sql查询

![new_cx](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/0.33/new_cx.png)