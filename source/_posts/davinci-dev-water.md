---
title: Davinci-二次开发系列04：自定义水印扩展
date: 2020-04-03 23:00:00
categories: 
 - [Davinci]
 - [可视化工具]
tags:
 - BI
 - Davinci
---

# 水印（watermark）

​	目前来看，水印已经具有很高的附加价值了，钱币的水印能防伪，资料上的水印能宣传，甚至很多对外的产品软件都把去水印当成VIP的一项特殊功能，随着知识产权加强，知识付费的流行，水印也成了防止盗用的一种很重要的手段。

​	Davinci的水印我觉着官方迟早会出，但目前来看短时间出不来，出成什么样子也不确定，所以我们就先制瓜吃瓜。

<!--more-->
​	效果：

![watermark](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/04/watermark.gif)

# 水印制作

## 自定义水印位置

​	既然是自定义水印，自然也离不开用户和权限，为了先实现自定义，不考虑复杂因素，自定义水印根据项目划分，支持快捷显示项目名称等，不考虑用户自定义和项目自定义水印同时存在的情况。

​	在用户界面》》组织》》项目》》设置》》水印设置

## 水印显示位置

目前来看有这么几个位置：

- widget编辑区（编辑区理论不需要水印，观点不一）
- dashboard看板
- display大屏
- widget分享面板（官方留了口子，但没有实现完善，暂不考虑）
- dashboard分享面板
- display分享面板

### widget工作区：

![water-edit](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/04/water-edit.jpg)

### dashboard页面：

![water-dashboard](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/04/water-dashboard.jpg)

### display页面：

![water-display](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/04/water-display.jpg)

## 水印属性

- 开启关闭

- 项目名称

- 用户名

- 水印文本（比如“内部资料，严禁外传”这类）

- 水印时间戳（支持多种日期格式）

- 水印颜色

备注：未实现上传图片等格式的水印

![data](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/04/data.jpg)

# 水印编码实现

## 用户信息获取

​	登录用户信息获取目前有两种

​	方式一：

```
const mapStateToProps = createStructuredSelector({
  loginUser: makeSelectLoginUser(),
})
```

​	方式二：

```
const loginUser = localStorage.getItem('loginUser')
```

## 项目信息获取

```
const mapStateToProps = createStructuredSelector({
  currentProject: makeSelectCurrentProject()
})
```

## 后台改造

后台改造比较简单：

第一：project表增加config字段，类型text，注意和json的转换

第二：分享功能增加project信息，用于展示项目名称，因为分享不登录状态即可访问，所以无法显示用户名

## 前端改造

前端改造难点除代码语法外，

第一：重点还是在用户名称和项目名称获取上

第二：自定义水印在组织里面，项目的获取和值传递

第三：分享功能，项目信息传递

第四：水印组件引入可以放在多个位置，放到widget下面会比较省劲，尤其是要求多个地方都呈现水印

# 几个推荐和注意

## dashboard和display水印问题

​	第一：看上面的效果图能明显看到水印是最终实在单个widget下面的，而不是在整个面板上的，整体效果就差一些

​	第二：尤其是大屏这种，可以考虑每个大屏自定义水印，因为大屏有自身特有的组件

## 导出excel水印

数据导出excel同样需要水印，推荐实现

## 分享链接的水印

Davinci里面widget、dashboard、display都支持分享，widget不完善，分享不受权限控制，也不是我们正常进入到某个项目里面的操作，而我们的自定义水印是根据项目设定的，所以分享链接需要根据dashboard获取项目信息和水印的配置信息，并传递到widget组件里。

## 定时任务邮件接收水印

邮件接收报表带水印是非常合理的，根据水印时间戳更是能够准确判断当前报表的出具时间。

# 交流学习

刚建的群，学习Metabase、Davinci等开源BI，群号：72569367，感兴趣的可以加一下。