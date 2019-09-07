---
title: 【BLOG】hexo搭建blog教程
date: 2019-08-16 13:46:30
categories: 
 - hexo
tags:
 - hexo
---

## 概述

[hexo]: https://hexo.io	"是快速、简洁且高效的博客框架"

特点：

1. nodejs生成搭建快速
2. 支持markdown
3. git一键部署
4. 插件丰富，生态完善

<!--more-->

## 环境准备

建议使用vscode编辑器，依据个人喜好而定。

- 安装nodejs
- 安装git
- 安装hexo

```
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
$ hexo server
```

文档结构

```
.
├── node_modules
├── scaffolds
├── source
| └── _posts
├── themes
├── _config.yml
├── package.json
├── package-lock.json
└── yarn.lock
```

- 启动

```
$ hexo server
```

- 访问地址

http://localhost:4000/

本地blog搭建完成

## 常用命令

- 生成静态文件

```
$ hexo generate
```

简写

```
$ hexo g
```

| 参数             | 描述                   |
| :--------------- | :--------------------- |
| `-d`, `--deploy` | 文件生成后立即部署网站 |
| `-w`, `--watch`  | 监视文件变动           |

- 清除缓存文件（更换主题等操作使用）

```
$ hexo clean
```

- 发布部署

```
$ hexo deploy
```

简写

```
$ hexo d
```

| 参数               | 描述                     |
| :----------------- | :----------------------- |
| `-g`, `--generate` | 部署之前预先生成静态文件 |

其他命令，见[hexo]: https://hexo.io 官网



## 主题设置

[hexo]: https://hexo.io 官网上有很多主题，这里我们采用

[next]: http://theme-next.iissnan.com

主题

在终端窗口下，定位到 Hexo 站点目录下。使用 `Git` checkout 代码：

```
$ cd your-hexo-site
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```



## Live 2D模型插件

安装npm包

```
npm install --save hexo-helper-live2d
```

在hexo的站点配置文件`_config.yml`中添加如下配置

```
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  debug: false
  model:
    use: live2d-widget-model-模型名称
  display:
    position: right
    width: 150
    height: 300
  mobile:
    show: true
```

下载模型

```
npm install live2d-widget-model-模型名称
```

模型列表：

`live2d-widget-model-chitose`
`live2d-widget-model-epsilon2_1`
`live2d-widget-model-gf`
`live2d-widget-model-hibiki`
`live2d-widget-model-haru/01 (use npm install --save live2d-widget-model-haru)`
`live2d-widget-model-haru/02 (use npm install --save live2d-widget-model-haru)`
`live2d-widget-model-haruto`
`live2d-widget-model-hijiki`
`live2d-widget-model-izumi`
`live2d-widget-model-koharu`
`live2d-widget-model-miku`
`live2d-widget-model-ni-j`
`live2d-widget-model-nico`
`live2d-widget-model-nietzsche`
`live2d-widget-model-nipsilon`
`live2d-widget-model-nito`
`live2d-widget-model-shizuku`
`live2d-widget-model-tororo`
`live2d-widget-model-tsumiki`
`live2d-widget-model-unitychan`
`live2d-widget-model-wanko`
`live2d-widget-model-z16`

## 来必力评论插件

登录来必力注册账号，在代码管理里面找到 data-uid。

到主题配置文件`_config.yml`里，添加 livere_uid。

备注：如果想取消某个页面的评论，在文章属性中增加comments：false即可

## DaoVoice在线聊天插件

首先需要注册一个 DaoVoice，[点击注册](http://dashboard.daovoice.io/get-started?invite_code=7f3d6e70)

应用设置》安装到网站

以 next 主题为例，打开 `themes/next/layout/_partials/head.swig` 文件中添加代码，位置随意：

```
{% if theme.daovoice %}
  <script>
  (function(i,s,o,g,r,a,m){i["DaoVoiceObject"]=r;i[r]=i[r]||function(){(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;a.charset="utf-8";m.parentNode.insertBefore(a,m)})(window,document,"script",('https:' == document.location.protocol ? 'https:' : 'http:') + "//widget.daovoice.io/widget/0f81ff2f.js","daovoice")
  daovoice('init', {
      app_id: "{{theme.daovoice_app_id}}"
    });
  daovoice('update');
  </script>
{% endif %}
```

可以在DaoVoice官网上，设置按钮，绑定微信等操作

在主题配置文件 `_config.yml`，添加如下代码：

```
# Online contact 
daovoice: true
daovoice_app_id: 这里输入前面获取的app_id
```

## 搜索服务插件

安装 `hexo-generator-searchdb`

```
$ npm install hexo-generator-searchdb --save
```

站点配置文件，增加如下内容

```
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

修改主题配置文件，启用搜素功能

```
# Local search
local_search:
  enable: true
```

## 百度统计插件

登录[百度统计](http://tongji.baidu.com/) ，添加站点，获取代码，找到ID

```
hm.src = "https://hm.baidu.com/hm.js?百度统计ID";
```

主题配置文件`themes/*/_config.yml`，修改字段 `google_analytics`

```
# Baidu Analytics ID
baidu_analytics: 百度统计ID 
```

[百度分享](http://share.baidu.com/)

百度分享服务，主题配置文件，修改字段 `baidushare`，值为 `true`即可。

```
# Baidu Share
# Available value:
# button | slide
# Warning: Baidu Share does not support https.
baidushare: true
```

[need-more-share2](https://github.com/revir/need-more-share2)

主题配置文件，修改字段 `needmoreshare2`，值为 `true`即可

```
needmoreshare2:
  enable: true
```

## RSS配置

```
简易信息聚合（也叫聚合内容）是一种RSS基于XML标准，在互联网上被广泛采用的内容包装和投递协议。RSS(Really Simple Syndication)是一种描述和同步网站内容的格式，是使用最广泛的XML应用。RSS搭建了信息迅速传播的一个技术平台，使得每个人都成为潜在的信息提供者。发布一个RSS文件后，这个RSS Feed中包含的信息就能`直接被其他站点调用`，而且由于这些数据都是标准的XML格式，所以也能在其他的终端和服务中使用，是一种描述和同步网站内容的格式。——百度百科
```

安装`hexo-generator-feed`

```
$ npm install --save hexo-generator-feed
```

站点配置文件 ，Extensions，添加配置

```
# Extensions
## Plugins: https://hexo.io/plugins/
Plugins: hexo-generate-feed
```

主题配置文件，字段rss，添加配置

```
rss: /atom.xml
```

具体内容需要烧制feed

侧边栏会生成一个RSS图标

## 部署发布

你可以自己搭建服务器部署，但是会很麻烦，这里我们可以采用主流git代码托管网站里发布自己的blog

- github page

- coding
- 码云
备注：README的问题

  第一：执行命令hexo g之后，会把source文件里的.md格式的文件渲染为html文件并放到public下面

  第二：执行命令hexo d之后，会把public下面的所有文件放到.deploy_git并提交到对应的github这个仓库

  第三：hexo进行渲染文件时渲染不到README文件，所有就进不到部署路径里面

  解决办法：

  - 我们在本地的source文件里新建一个README.md文件。

  - 修改Hexo根目录下的_config.yml文件，将skip_render参数的值设置为README.md

    ```
    skip_render: README.md
    ```

## Travis CI

​	Travis CI 提供的是持续集成服务（Continuous Integration，简称 CI）。我们在软件开发过程中，有构建、测试、部署这些必不可少的步骤，而这些会花掉我们很多的时间。为了提高软件开发的效率，现在涌现了很多自动化工具。Travis CI 是目前市场份额最大的一个，而且有很详细的文档以及可以和 Github 很好的对接。

​	详见我的blog文章。
## 更换hexo网页图标

制作favicon图标

1. 准备图片

2. 搜索*favicon 在线*，在线图片转favicon的工具，推荐使用： [bitbug](http://www.bitbug.net/)

3. 生成图标16x16，32x32两种。

在主题配置文件`themes/*/_config.yml`修改

```
favicon:
small: /images/favicon-16x16-next.ico
medium: /images/favicon-32x32-next.ico