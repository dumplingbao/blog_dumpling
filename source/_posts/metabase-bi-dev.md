---
title: Metabase-BI系列：二次开发环境(windows)搭建
date: 2019-11-04 23:54:03
categories: 
 - [Metabase]
 - [可视化工具]
tags:
 - BI
 - Metabase
---
## 概述

[Metabase](https://link.jianshu.com/?t=http://www.metabase.com/) 官网介绍：是一种简单、开源的方式，让公司中的每个人都可以通过它来提问和学习数据。

如果了解更多BI工具，可看我的另一篇文章：BI、数据可视化工具浅析整理。
<!--more-->
Metabase特点：

- 开源免费
- 适合看运行数据： 提问的方式，业务人员自己就可以做数据分析
- 报表自动化，数据可视化
- 权限管理控制
- 数据共享，操作简单
- 可以与ETL结合
- 支持与业务系统做集成

## 二次开发

前端框架：React + Redux等相关框架，基于yarn的开发环境，webpack构建

后端开发：metabase后端语言采用clojure，Ring和toucan等开发框架

补充：

前端框架：React + Redux + D3（图表工具）

后端框架：Clojure + RING（中间件） + Compojure（路由框架） + Toucan（ORM框架）

## 源代码

github地址： https://github.com/metabase/metabase 

安装git工具，clone源代码

## 开发工具

- idea 需安装clojure相关插件， Cursive和Leinigen 
- vscode 安装 calva和clojure插件

## 安装工具

### Leiningen

Leiningen用于自动化Clojure项目，例如创建项目、抓取依赖包、编译、运行、测试、此外，Leiningen可以为项目生成maven风格的“pom”文件以进行互操作。

安装过程：

第一步：

官网下载[zip包](https://github.com/technomancy/leiningen/releases)和[bat文件](https://leiningen.org/#install)

第二步：

自定义Leiningen安装目录，并在安装目录下面，创建bin文件夹和self-installs文件夹

将bat文件，lein.bat放到bin目录下面

将zip包修改后缀为jar，然后放到self-installs文件夹中，如：leiningen-2.9.1-standalone.jar

备注：也可以直接找安装版安装即可

第三步：

配置环境变量，创建LEIN_HOME变量，配置%LEIN_HOME%\bin目录到path中

运行命令lein repl :start，该命令会先启动REPL服务端，接着启动REPL客户端连接所启动的REPL服务端

到此，Leiningen安装到了windows上

### yarn

下载地址：https://yarnpkg.com/en/docs/install

### nodejs

安装最新版本

## 部署发布

windows二次开发环境部署，采用yarn，不建议用npm，容易出问题，推荐编辑工具vscode

初始化

```
yarn install
```

前端编译

```
yarn run build
```

后端运行

```
lein run
```

访问默认端口3000

http://localhost:3000

有个配置引导，数据可以直接选稍后配置，其余下一步即可

![Mb-home](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/Metabase-home.jpg)

## Metabase自身默认数据库修改

Metabase默认使用的H2数据库，如果想改用本地mysql需要，修改环境变量（或者启动之前设置一下），如果直接运行不生效，建议重启一下电脑

```
set MB_DB_TYPE=mysql
set MB_DB_DBNAME=metabase
set MB_DB_PORT=3306
set MB_DB_USER=root
set MB_DB_PASS=123456
set MB_DB_HOST=localhost
```

注意一：myql数据库创建用utf-8，utf8mb4有可能出现问题

注意二：如果碰到 1709 – 索引列大小太大.最大列大小为767字节问题

```
//查看
SHOW VARIABLES LIKE 'storage_engine';//默认应该是MyISAM，修改为InnoDB，这个修改最好改配置文件
SHOW variables like 'innodb_large_prefix'
SHOW variables like 'innodb_file_format'
//修改
SET GLOBAL INNODB_LARGE_PREFIX = ON;
SET GLOBAL INNODB_LARGE_PREFIX = ON;
SET GLOBAL innodb_file_format = BARRACUDA;
```

## Metabase默认DEMO数据库+H2工具

Metabase默认是数据库是H2数据库，连接工具直接去[H2](http://www.h2database.com/html/main.html)官网下载即可

连接配置，你可以直接到Metabase本身数据里面去找，表：metabase_database

```
{"db":"/E:/XXXX/metabase-master/resources/sample-dataset.db;USER=GUEST;PASSWORD=guest"}
```

可以直接看到数据库，用户名和密码，注意配置连接直接访问数据库文件就可以，不能多个应用同时访问

![H2-login](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/H2/H2-login.jpg)

![H2-home](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/H2/H2-home.jpg)

## 常用命令

lein repl：打开REPL环境。

前端编译：yarn build

前端热部署：yarn build-hot

后端运行：lein run

后端热部署：lein ring server

lein uberjar：打包项目，包含依赖项。得到jar后就跟平常的jar没有区别了。
