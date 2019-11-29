---
title: Metabase-BI系列04：cookie实现单点登录sso
date: 2019-11-29 23:27:12
categories: 
 - [Metabase]
 - [可视化工具]
tags:
 - BI
 - Metabase
---
## 概述

​    Metabase 可以作为独立的BI平台，本身就有用户组和权限组。而且Metabase支持报表的分享和iframe嵌入的方式进行报表的呈现，我们可以通过这种方式进行数据的呈现。

​    我们需要登录Metabase系统进行报表创建和发布，如果业务平台有权限的用户想通过业务平台用户进入到Metabase里面，就需要进行单点登录，因为用户不可能登录两个平台用两个账户，当然如果用户能够接受，那就可以用两个账户了。

​    Metabase支持多种单点登录（sso）方式：

<!--more-->
- Google账号登录，这种方式在中国只能呵呵了

-  LDAP的方式，需要搭建一个LDAP统一认证的服务，这个麻烦点，Metabase提供配置LDAP功能，比较方便

- 基于SAML协议接入单点登录， SAML即安全断言标记语言，这个不是很熟，可以尝试一下

- 通过cookie实现单点，操作简单，但是比较受限，安全性低

这里介绍通过cookie实现简单的单点登录

## Metabase-cookie

### metabase.SESSION_ID

首先看一下Metabase里面的cookie， metabase.SESSION就是Metabase访问权限的令牌，path为“/”根目录下面，所以获取令牌，将把令牌塞到metabase.SESSION里就可以了。

![Metabase-cookie](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/cookie/Metabase-cookie.jpg)

### /api/session/

Metabase doc API说明如下：

```
## `POST /api/session/`**
Login.
**##### PARAMS:**
\* ***\*****`username`*****\*** value must be a non-blank string.
\* ***\*****`password`*****\*** value must be a non-blank string.
```

所以，我们需要调用/api/session/，传入用户和密码，获取令牌

![cookie01](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/cookie/api-session.jpg)

![cookie02](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/cookie/cookie-id.jpg)

### 跨域问题

直接访问Metabase的/api/session会存在跨域的问题

![404](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/cookie/404.jpg)

![cors](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/cookie/cors.jpg)

修改Metabase后台允许跨域，Metabase后台用的Ring框架控制跳转，我们引入开源的[ring-cors]( https://github.com/r0man/ring-cors )框架，配置允许跨域的域和请求，配置Metabase的`handler.clj`

```
(require '[ring.middleware.cors :refer [wrap-cors]])
(def handler
  (wrap-cors my-routes :access-control-allow-origin [#"http://example.com"]
                       :access-control-allow-methods [:get :put :post :delete]))
```

### cookie共享问题

对于cookie来说，不同域下的cookie不共享，必须在同个顶级域下设置cookie，所以这也是这种cookie实现单点比较受限制的问题。

对于统一域名不同端口的情况，直接将cookie放到path根目录“/”即可，因为域是相同的

对于不同的二级域名的情况，放到顶级域名下即可

如：aa.test.com和bb.test.com

```
Cookies.set('metabase.SESSION', token, { path: '/',domain: '.test.com' })
```

关于cookie的介绍，推荐文章： https://www.cnblogs.com/hujunzheng/p/5744755.html 