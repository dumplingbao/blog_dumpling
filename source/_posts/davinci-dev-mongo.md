---
title: Davinci-二次开发系列08：mongoDB（JDBC查询）四种解决方案
date: 2020-08-24 12:00:00
categories: 
 - [Davinci]
 - [可视化工具]
tags:
 - BI
 - Davinci
---
​	关于Davinci使用mongoDB的问题，就一直没有断过。我们不管怎么评价mongoDB，似乎年轻的mongoDB俨然是主流数据库了。而且现在对于BI来说能不能支持主流的NoSQL数据库，已经成为一个很重要的衡量标准。目前BI通过JDBC的方式访问NoSQL数据库还是首选，但是mongo官方未提供JDBC驱动包，这里介绍四种mongo-JDBC查询数据的解决方案：（备注：mongo-JDBC问题不仅限于Davinci，也不限于BI，一些实际应用中也会碰到）

| 序号 | 方案                                     | 备注                    | 综述                                                         |
| ---- | ---------------------------------------- | ----------------------- | ------------------------------------------------------------ |
| 1    | 利用presto连接                           | facebook开源SQL查询引擎 | 优点：开源、免费、兼容性良好<br>缺点：需要单独部署服务       |
| 2    | 1、unityjdbc破解版<br>2、unityjdbc试用版 | 商业化产品              | 优点：集成简单、不需要单独部署<br>缺点：收费、_id字段值不显示<br>试用版本地mongo支持需要解决<br>破解版只能简单的查询、子查询等有问题，存在问题比较多 |
| 3    | mongo-bi连接器                           | mongo官方               | 优点：官方出品、免费、兼容性极好<br/>缺点：需要单独部署服务<br/>推荐用此方式 |
| 4    | 开源手写驱动包                           | 开源                    | 优点：灵活性可控<br/>缺点：成本高、耗精力                    |

<!--more-->
# 利用presto连接

​	Presto是一个facebook开源的分布式SQL查询引擎，而且支持跨库查询，当然也支持mongoDB，Davinci通过presto做类似的桥接，然后也能实现mongoDB的jdbc查询，如果公司使用presto，采用这种方案也是不错的选择，兼容性可以。

​	这里我们重点介绍后面两种，没有实际进行尝试，大家可以尝试一下，简单介绍一下步骤：

​	第一：首先需要安装presto-server服务，看好版本，支持MongoDB的才可以

​	第二：配置mongodb.properties

```
connector.name=mongodb
mongodb.seeds=ip:port
mongodb.schema-collection=admin
```

​	第三：Davinci配置数据源	

​	`jdbc:presto://ip:port/mongodb/test`

​	第四：view实现JDBC查询

​	`select * from mongodb.数据库名.集合`

# unityjdbc

​	由于[unityjdbc](http://www.unityjdbc.com/)是收费的，这里我们用网上的破解版和试用版都试一下。

## 官方试用版

​	第一：试用版，去官网[unityjdbc](http://www.unityjdbc.com/)下载`JDBC Driver for MongoDB`，这里我们用的是``这个版本下载地址：http://www.unityjdbc.com/download.php?type=mongodb

​	第二：下载下来的`UnityJDBC_Trial_Install.jar` 需要`java -jar`安装，安装完获取`unityJdbc.jar`（或者`mongodb_unityjdbc_full.jar这个jar`包也可以）

![mongo_uj00](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/08/mongo_uj00.jpg)

​	第三：`mongodb-driver`的依赖默认是注释掉的，将注释去掉，并将`unityJdbc.jar`引入到工程里面或者复制进去，在porm里面引入本地依赖

```
		<!--mongodb -->
		<dependency>
			<groupId>org.mongodb</groupId>
			<artifactId>mongodb-driver</artifactId>
			<version>3.6.3</version>
		</dependency>
		<dependency>
			<groupId>unityjdbc</groupId>
			<artifactId>unityjdbc</artifactId>
			<scope>system</scope>
			<systemPath>${project.basedir}/lib/unityjdbc.jar</systemPath>
		</dependency>
```

​	第四：Davinci配置数据源（强调一下，我用官方的包，连接本地mongo报hostname的错误，就先用官方的测试库）

`jdbc:mongo://localhost:27017/数据库` 

官方测试地址 `jdbc:mongo://ds029847.mongolab.com:29847/tpch`

![mongo-uj01](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/08/mongo_uj01.jpg)

### 创建view(_id不能正常显示）

![mongo_uj07](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/08/mongo-uj07.jpg)

### 创建widget

![mongo_uj08](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/08/mongo-uj08.jpg)

![mongo_uj09](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/08/mongo-uj09.jpg)

### 子查询

![mongo_uj10](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/08/mongo-uj10.jpg)

### 关联查询

![mongo_uj11](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/08/mongo-uj11.jpg)

## 网传破解版

试了一下分享出来的破解版，除去上面的安装步骤，直接引入即可

创建数据源，这个包比试用版好的是本地mongo能直接使用

`jdbc:mongo://localhost:27017/数据库`

### 创建view（_id不显示、集合列表不显示）

![mongo_uj02](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/08/mongo_uj02.jpg)

### 创建widget

![mongo_uj03](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/08/mongo_uj03.jpg)

从错误上来就不能很好的兼容，子查询关联查询更是不能正常使用

# mongo-bi连接器

​	mongo官方没有提供mongo的JDBC驱动，但是提供了mongo-bi-connector，允许使用所选的BI工具通过标准SQL查询对MongoDB数据进行可视化。

​	搜了一下资料，BI Connector 可以使用 SQL 或 ODBC 数据源方式直接访问 MongoDB，MongoDB 早期版本直接使用 Postgresql FDW 来实现 SQL 到 MQL 的转换，后来实现更加轻量级的 mongosqld 来支持 BI 工具的连接。这里我们就尝试一下mongosqld，来验证一下。

### 下载并安装mongo-bi-connector

​	官方下载：https://www.mongodb.com/try/download/bi-connector，支持windows、linux版本

​	默认安装即可

### 配置文件

​	默认配置即可，如果有mongo做了限制，或者调整端口，修改`mongosqld-config.yml`，详细配置看官网

```
net:
  bindIp: "127.0.0.1" # To bind to multiple IP addresses
  port: 3307
  mongodb:
  net:
    uri: "mongodb://127.0.0.1:27017"
    ssl:
      enabled: false
    auth:
      username: xxx
      password: xxx
      source: xxx
      mechanism: SCRAM-SHA-1
```

### 启动mongosqld

​	我使用的windows版本双击`mongosqld.exe`运行即可

### maven引入依赖包

```
<!-- 这个包必须有 -->
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
	<version>5.1.44</version>
</dependency>
<!-- 非必须，这个是JDBC认证插件，如果你做测试或者你的mongo本身在裸奔，就没必要了 -->
<dependency>
   <groupId>mysql</groupId>
   <artifactId>mysql-connector-java</artifactId>
   <version>5.1.42</version>
</dependency>
```

### 创建数据源

备注：这里使用mysql即可

![mongo-bi01](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/08/mongo-bi01.jpg)

### 创建view（_id和集合列表均正常）

![mongo-bi02](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/08/mongo-bi02.jpg)

### 创建widget

![mongo-bi03](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/08/mongo-bi03.jpg)

![mongo-bi04](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/08/mongo-bi04.jpg)

### 子查询

![mongo-bi05](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/08/mongo-bi05.jpg)

### 关联查询

![mongo-bi06](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/davinci/08/mongo-bi06.jpg)

# 开源手写驱动包

​	这个绝对是一个解决方案，其实工作中就会封装mongo的一些比如关联查询类似的组件，写这个驱动包也未必没有优势，我觉着如果上面的方案兼容sql的场景如果存在问题，那么自己写的驱动包改造就容易的多，第三方的包就没那么好改了。

​	这里推荐一个mongo的JDBC开源驱动包，如果有自己想写的想法仅供参考。

​	连接：https://gitee.com/f4haofeng/mongodb-jdbc/tree/master/src/main/java/com/mongodb

# 综上所述

​	其实，表格里面已经比较了，根据个人需求选择即可，还是推荐使用mongo官方的BI连接器的方式，虽然我们没有对性能进行比较，但从使用性、兼容性、子查询、关联查询等方面比较，BI连接器的方式完全能够胜任。

# 交流学习

学习Metabase、Davinci等开源BI，群号：72569367，感兴趣的可以加一下。
