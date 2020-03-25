---
title: Metabase-BI系列11：看板模式扩展及使用
date: 2020-03-24 12:00:00
categories: 
 - [Metabase]
 - [可视化工具]
tags:
 - BI
 - Metabase
---

## 两种模式

​	最近微信的暗黑模式似乎比较火，为大家熬夜又增添一些乐趣，这次说一下Metabase里面的扩展新的模式，熟悉Metabase的人都知道，Metabase的看板dashboard有两种模式：`白天模式`、`夜间模式`

<!--more-->
白天模式：

![white](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/03/white.jpg)

夜间模式：

![night](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/03/night.jpg)



- 由于Metabase BI开源版本只支持白天、夜晚两种模式，现改造可进行定制开发，并支持切换多模式。
- Metabase BI主题是采用前端参数配置，未将主题设置保存数据库，保留原有设计，通过参数控制主题显示。
- 后期根据用户需求进行定制改造即可

## 如何扩展模式呢

### 新增模式

修改文件 `/metabase/frontend/src/metabase/components/icons/NightModeIcon.jsx` 

```
const OPTIONS = [
 { name: t`白天模式`, theme: "sun" },
 { name: t`夜间模式`, theme: "moon" },
 { name: t`蓝色模式`, theme: "blue" },
 { name: t`主题名称`, theme: "主题标识" },
];
```



### 模式图标定制

Metabase主推图标icon采用的svg path方式，修改文件`/metabase/frontend/src/metabase/icon_paths.js`

例如：

```
moon:
  "M11.6291702,1.84239429e-11 C19.1234093,1.22958025 24.8413559,7.73631246 24.8413559,15.5785426 C24.8413559,24.2977683 17.7730269,31.3660972 9.05380131,31.3660972 C7.28632096,31.3660972 5.58667863,31.0756481 4,30.5398754 C11.5007933,28.2096945 16.9475786,21.2145715 16.9475786,12.9472835 C16.9475786,7.90001143 14.9174312,3.32690564 11.6291702,1.70246039e-11 L11.6291702,1.84239429e-11 Z",
新增主题标识:
  "M16 0c-8.837 0-16 7.163-16 16s7.163 16 16 16 16-7.163 16-16-7.163-16-16-16zM16 30c-1.967 0-3.84-0.407-5.538-1.139l7.286-8.197c0.163-0.183 0.253-0.419 0.253-0.664v-3c0-0.552-0.448-1-1-1-3.531 0-7.256-3.671-7.293-3.707-0.188-0.188-0.442-0.293-0.707-0.293h-4c-0.552 0-1 0.448-1 1v6c0 0.379 0.2144.208z",
```



### 模式样式修改

修改文件`/metabase/frontend/src/metabase/css/dashboard.css`

样式涉及dashboard背景、标题，card图例、标题、背景等样式，所以将各样式设置Dashboard--+主题标识的方式

```
/* moon mode start*/
.Dashboard.Dashboard--主题标识{
 background-color: var(--color-bg-black);
}
.Dashboard.Dashboard--主题标识 .DashboardHeader {
 color: var(--color-text-white);
}
.Dashboard.Dashboard--主题标识 .Card {
 color: var(--color-text-white);
}
.Dashboard.Dashboard--主题标识 .Header-button,
.Dashboard.Dashboard--主题标识 .Header-button svg {
 color: color(var(--color-text-medium) alpha(-70%));
}
.Dashboard.Dashboard--主题标识.Dashboard--fullscreen .fullscreen-night-text {
 color: color(var(--color-text-white) alpha(-14%));
 transition: color 1s linear;
}
.Dashboard.Dashboard--主题标识 .DashCard .Card svg text {
 fill: color(var(--color-text-white) alpha(-14%)) !important;
}
.Dashboard.Dashboard--主题标识 .DashCard .Card {
 background-color: var(--night-mode-card);
 border-color: var(--night-mode-card);
}
.Dashboard.Dashboard--主题标识 .enable-dots-onhover .dc-tooltip circle.dot:hover,
.Dashboard.Dashboard--主题标识 .enable-dots .dc-tooltip circle.dot {
 fill: currentColor;
}
/* moom mode end*/
```

## 模式使用

###  模式切换

重新打包发布应用，重启应用程序，web端进入dashboard编辑或者预览的全屏模式先，点击切换模式即可

原模式切换：

![old_theme](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/03/old_theme.jpg)

改造后模式切换：

![new_theme](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/03/new_theme.jpg)

### 参数配置化

url+#theme=模式标识

如：http://localhost:3000/dashboard/10#theme=blue

![blue](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/03/blue.jpg)

## 注意点

源码中模式切换是`Boolean`类型，如果改成多模式，就不止两种类型，所有需要改成`String`类型

## 交流学习

刚建的群，学习Metabase、Davinci等开源BI，群号：72569367，感兴趣的可以加一下。


