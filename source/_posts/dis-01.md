---
title: 开源论坛：社区选择Discourse
date: 2021-03-02 23:00:00
categories: 
 - discourse
tags:
 - discourse
---

回想以往逛论坛、贴吧的年代，似乎渐行渐远，现在人类追求的多样性超出想象，但论坛并未走下神坛，只是换了一种或者是以多种方式存在。社区、小组、圈子等都有更多的载体和工具，今天就说一下开源论坛discourse。

Discourse应该也已经20多年了，并不新鲜，也是因为平常经常浏览的技术社区用的就是discourse，所以就搭建一下，深入感受一下discourse，以备后用。
<!--more-->
Discourse总体下来，觉得挺适合做社区，尤其是技术性比较聚集的论坛

- 开源论坛
- 创始人想要让改变十年未变的互联网论坛模样
- 基于Ruby on Rails 和 Ember.js 开发，数据库使用 PostgreSQL 和 Redis
- 最大的特点是简洁和专业性，以话题为关系聚集用户

![dis](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/2021/dis/dis.png)

## Discourse官网：

- 官方网站：https://www.discourse.org/
- 官方社区：https://meta.discourse.org/
- Github项目：https://github.com/discourse/discourse

## 搭建步骤

### 安装git和Docker

```
apt-get install git
wget -qO- https://get.docker.io/ | sh
```

### 安装Discourse

```
mkdir /var/discourse
git clone https://github.com/discourse/discourse_docker.git /var/discourse
cd /var/discourse
cp samples/standalone.yml containers/app.yml（or ./discourse-setup 初始化）
```

### 修改配置文件

```
vim containers/app.yml

UNICORN_WORKERS（如果是1Gb内存就是2，2GB内存以上就是3-4）
DISCOURSE_DEVELOPER_EMAILS管理员邮箱
DISCOURSE_HOSTNAME 绑定的域名
DISCOURSE_SMTP_ADDRESS是邮局服务器
DISCOURSE_SMTP_PORT是SMTP的端口
DISCOURSE_SMTP_USER_NAME账号
DISCOURSE_SMTP_PASSWORD密码
```

特别注意的是：端口号465是没有效果的

```
DISCOURSE_SMTP_PORT: 587
```

以上配置完了，会收不到邮件有两种方式修改

第一种：用官方的工具launcher创建管理员账号

```
cd /var/discourse
./launcher enter app
rake admin:create
```

- 创建管理员账号，按要求输入管理员邮箱和登录密码
- 登录网站，用刚才创建的账号直接登录。
- 在settings页面设置notification email为发件邮箱，就是之前配置文件里面写的那个邮箱。
- 在邮件测试页面发一封测试邮件，应该测试成功了。

第二种：编辑发件邮箱

```
vim containers/app.yml
//定位文件底部，打开注释
- exec: rails r "SiteSetting.notification_email='xxx@qq.com'"
//重新build一下
./launcher rebuild app
```

### 修改完成后，重新构建

```
./launcher rebuild app
```

### nginx与discourse

discourse配置修改：

```
cd /var/discourse
vim containers/app.yml
```

修改：(设置端口代理)

```
expose:
  - "9090:80"   # http
```

最后运行

```
./launcher rebuild app
```

### nginx增加配置：

增加discourse.conf即可

### 添加Github第三方登录

打开[Github application](https://github.com/settings/applications/)，进入Developer applications，新建应用程序

Homepage url为你的discourse地址，authorization callback url为你的discourse地址加上/auth/github/callback，然后复制Client ID和Client Secret，
最后打开discourse，进入管理－设置－登录，找到下图所示三个字段，勾选填入刚才复制的内容

然后确定、完成。

点击=>完成！

### 常见问题

计算技术论坛关于discourse的问题：https://www.ossez.com/search?q=discourse

## 其它

体验了一下Discourse的管理界面和可配置的操作，后台支持定制化的配置和插件化，而且确实很简洁，并能够提供对外API，有时间可以继续探究

我的discourse地址：https://lt.disscode.cn/
