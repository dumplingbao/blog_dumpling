---
title: BI、数据可视化工具浅析整理
date: 2019-11-03 16:21:04
categories: 
 -[Metabase]
 -[可视化工具]
tags:
 - BI
 - Metabase
---
AI（ Artificial Intelligence ）：人工智能

BI（ Business Intelligence ）：商业智能

BI商业智能，个人感觉商业化了一些，而且感觉没有把数据的价值体现的名称上，也不知道为什么不直接一点叫DI（Data Intelligence ）。

AI和BI似乎不可分割，起码相辅相成。AI似乎更火热，BI提出更早一些，似乎有点过气和传统的感觉，但是想想，可视化数据展示、数据可视化，这些名词或许目前更流行，其实应该都算是BI的变种，个人感觉似乎又有点去商业化，起码不叫什么商业可视化。

阿里云推出了大数据和人工智能DataV数据可视化、 Quick BI等等产品，且不说做到什么程度，加上手机、大屏的时代发展，就会发现数据的价值也越来越大。

由于工作对BI牵扯甚多，近来使用Metabase，准备写一下Metabase系列，就已BI、数据可视化做引子，查找资料总结了一下，后续会更新修改。这里不讲AI，不谈商业，通过搜索网上资料进行整理，就说一下接触的或者市面上BI、数据可视化的工具。

这里进行分类：重量级（商用+服务）、轻量级（开源）、商业级（服务），其它（心血来潮的一些小项目：GitDataV、DataVisualization）
<!--more-->

## 重量级（商用+服务）

### IBM Cognos

IBM Cognos 起初是加拿大的一家公司，后来被IBM收购。功能非常强大，可以自身创建package、立方体数据模型，通过ETL工具进行数据清洗，然后定时抽取IBM Cognos模型，自身实现配置化集群和负载均衡等功能，能够处理大规模，多系统（财务、资金、业务、人力资源等等）数据，进行数据整合，配套工具完善且非常多，此外

- 扩展性比较强，尤其对多系统数据处理
- 权限、数据集成完善
- 网关、应用服务器、内容管理器组件灵活
- 比较重，数据量小就大材小用，一般建行等大企业用多
- 不知道现在是否支持nosql类型的数据库
- 支持导出备份及导入等操作

1、技术架构：java + BS架构

2、适用范围：

​	政府单位、大型企业

3、安装部署：

​	weblogic、websphere、tomcat等集成部署

4、数据源：oracle、db2、mysql关系型数据库

5、可视化：

- 支持各种图表展示
- 支持各种下钻及功能性扩展
- 展示组件强大

6、支持文档：

- 安装文档齐全
- 集成部署补丁包完善
- 资料论坛可查找资料，因为大型企业使用，所以个人分享的资料较少

7、权限管理：支持权限控制

8、二次开发：利用cognos自身特性进行扩展及集成开发，源代码不开放

![cognos](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/bi/cognos.png)

### 赛思

赛思是国内用的比较多的BI工具，是武汉东方赛思软件公司开发的，目前政府、金融等单位使用的多。赛思大数据平台，为政府部门、企业或是IT公司的大数据项目提供全方位的平台支持。也有数据的提取、加工、调度等强大的功能。服务团队规模有保障，解决方案完善，但是目前对nosql数据库支持欠缺。

### 其他

- Tableau
- Microsoft (Power BI)
- SAS (Visual Analytics)
- AWS(Athena and Kinesis)
- Google (Dataproc and Big Query)
- Fusionex (Giant) 
- Zendesk (Zime) 
-  SAP（BO）
-  Oracle（BIEE）

## 轻量级（开源）

### Superset

github地址： https://github.com/apache/incubator-superset 

1、技术架构：

后端：Python + Flask（ Web 应用框架 ） +SQLAlchemy（orm框架）

前端：React + Redux +D3

2、适用范围：

- 开发/分析人员做好看板，业务人员浏览看板数据

- 业务人员可自行编辑图表，查看满足条件的结果


3、安装部署：

​	docker方式的安装部署最简单

4、数据源：支持各种数据源，包括Hive、Kylin等

5、可视化：

- 支持的图表类型多，达47种

- 图表可视化选项少，例如，数据格式选项偏少，如需添加，需要修改配置文件

- 可在看板中添加筛选框，支持在不同条件下查看

- 不支持图表和看板分组管理

- 没有提供图表的下钻功能，不支持多图表间的复杂联动

- 不支持跨库的表关联查询
- 支持其他图标库扩展

6、支持文档：

- 安装部署和快速入门方面的文档详细

- 但具体功能和图表制作方面的介绍文档需要搜索资料
- 整体文档资料相当简陋

7、邮件通知：不支持

8、权限管理：

- 报表权限设置较复杂、繁琐

- 可实现对菜单、数据源、数据表、字段、图表、看板等权限控制


10、二次开发：

- 支持 RESTful API

- 原属Airbnb的开源项目，有大公司团队维护，版本更新、Bug修复、二次开发有较大保障
- 也有说代码维护迭代，不活跃

### Redash

github地址： https://github.com/getredash/redash 

1、技术架构：Python + Flask + AngularJS + SQLAlchemy

2、适用范围：由于是对SQL查询结果进行可视化，需要开发/分析人员做好看板，业务人员浏览看板数据。

3、安装部署：

- 安装部署相对较麻烦

- 参考部署文档


4、数据源：支持数据源比superset少，不支持Kylin

5、可视化：

- 支持的图表类型不如Superset多，仅12种

- 图表可视化选项多

- 不支持在看板种添加筛选框

- 不支持图表和看板分组管理

- 没有提供图表的下钻功能，不支持多图表间的复杂联动

- 不支持跨库的表关联查询


6、支持文档：

- 提供快速入门教程

- 每一个功能模块都有文档且条理清晰


7、邮件通知：支持定时发送邮件

8、权限管理：权限设置简单，仅控制用户组对数据源的权限（只有两个权限：Full access或View only）

9、二次开发：

​	提供完整的 RESTful API 接口

10、源代码：代码质量比Superset要好，但比Metabase差一点

### Metabase

github地址： https://github.com/metabase/metabase 

1、技术架构：

前端框架：React + Redux + D3（图表工具）

后端框架：Clojure + RING（中间件） + Compojure（路由框架） + Toucan（ORM框架）

2、适用范围：

​	界面漂亮、友好，使用体验好，适合业务人员使用

3、安装部署：

- windows下安装部署非常简单
- docker部署简单

4、数据源：支持数据源少（12种），不支持Hive、Kylin（硬伤）

5、创建步骤：连接数据源-->图表-->看板-->定时任务

6、可视化：

- 支持的图表类型不如superset多，仅14种

- 图表可视化选项多，例如，提供数据格式多，设置灵活

- 可在看板中添加筛选框，支持在不同条件下查看

- 通过创建集合，支持图表、看板、定时任务分组管理

- 提供图表的简单钻取功能，不支持图表间的复杂联动

- 不支持跨库的表关联查询


7、支持文档：

​	安装部署、快速入门、具体功能、API等方面的文档详细

8、邮件通知：支持定时发送邮件

9、权限管理：

- 权限设置单一，只有访问权限

- 仅实现对数据源、数据表、图表、集合等权限控制


10、二次开发：提供完整的API文档，即使完全不会 Clojure，依然可以凭借丰富的 API 与文档完成许多二次开发。

11、源代码：

- 代码质量最好，结构清晰，整洁度高
- clojure语法，函数式编程，学习成本较高

### Zeppelin

github地址：https://github.com/apache/zeppelin

严格意义上说，Zeppelin更像是一个notebook，而不是一个单纯的BI工具，来自Apache项目

1、技术架构：

​	交互式数据分析开源框架，支持多种语言， 包括Scala、Python、SparkSQL、Hive、Markdown、Shell等 

2、适用范围：似乎更适合开发人员

3、可视化：不支持sql查询

### SQLPad

github地址：https://github.com/rickbergfalk/sqlpad

SQLPad是一个基于Nodejs开发的直接在浏览器运行SQL查询并对结果进行可视化展示工具 

1、适用范围：适合开发人员

2、数据源： MySQL, Postgres, SQL Server, Vertica, Crate, Presto等 

3、可视化：特别支持sql，与Zeppelin不同，看名字就能看出来

### CBoard

github地址：https://github.com/yzhang921/CBoard

1、技术架构：

后端：Spring+MyBatis 

前端： ngularJS1和Bootstrap 

2、特点：

- 国人开发的一款可视化工具
- 交互设计的不错，但是感觉有点奇怪
- Java系

### Davinci

github地址： https://github.com/edp963/davinci 

1、技术架构：宜信开发的达芬奇，Java

2、可视化：功能还是比较全面的，只是在国内还没有大范围的使用



## 商业级（服务）

### FineBI

1、技术架构：java开发

后端：spring mvc + Hibernate

前端：fineui

2、适用范围：

- 开发/数据人员准备好数据，数据人员/业务人员分析。

- 业务人员完全可自行分析、制作可视化。整个数据分析流程分工明确。


3、安装部署：

​	直接官网下载电脑适配的版本安装激活即可

4、数据源：支持各种数据源，Apache Kylin、Derby、HP Vertica、IBM DB2、Informix、Sql Server、MySQL、Oracle、Pivotal Greenplum Database、Postgresql、ADS、Amazon Redshift、Apache Impala、Apache Phoenix、Gbase 8A、Gbase8S、Gbase 8T、Hadoop Hive、Kingbase、Presto、SAP HANA、SAP Sybase、Spark、Transwarp Inceptor、Hbase等主流的一些关系型数据库及非关系数据库MongoDB等

5、可视化：

- 支持的图表类型多，达47种

- 图表可视化选项少，例如，数据格式选项偏少，如需添加，需要修改配置文件

- 可在看板中添加筛选框，支持在不同条件下查看

- 不支持图表和看板分组管理

- 没有提供图表的下钻功能，不支持多图表间的复杂联动

- 不支持跨库的表关联查询


6、支持文档：

- 安装部署和快速入门方面的文档详细，还有教学视频

- 但具体功能和图表制作方面的介绍文档几乎没有


7、邮件通知：支持

8、权限管理：

- 有一套完整的数据、业务包、报表、人员部门权限管理，有流程节点。

- 可实现数据源、数据表、字段、图表、看板等权限控制


9、二次开发：

- 不支持java层面的开发

- 只有web接口

- 能与.NET集成、JBPM工作流集成、CAS单点登录


10、源代码：不公开，商业产品团队运营。

### DataV（阿里云）

DataV旨让更多的人看到数据可视化的魅力，帮助非专业的工程师通过图形化的界面轻松搭建专业水准的可视化应用，满足您会议展览、业务监控、风险预警、地理信息分析等多种业务的展示需求。

1、配备多种场景模板

2、多种图表组件支持

3、支持多种数据源

4、配置比较快速灵活

 ![datav01]( http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/0287021751/p7711.gif )

![datav02]( http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/3287021751/p7714.png )

### Sugar（百度）

连接地址： https://cloud.baidu.com/product/sugar.html 

### 小马BI

连接地址： https://xiaoma.qq.com/#/ 

### 网易有数

连接地址：https://bigdata.163yun.com/ 

## 其它

### GitDataV

github地址： https://github.com/HongqingCao/GitDataV 

 GitDataV，是一个github“大数据可视化平台”，通过它你可以更直观的看到你在github里的一些数据：
个人信息(✔)，仓库stars情况(✔)，仓库语言分类(✔)
仓库公开数量(✔)、粉丝数量(✔)、跟随数量(✔)、仓库数据(✔)、最近你的操作(✔)
最近的粉丝(✔)、最近的跟随(✔)、最新信息(✔)
左上角箭头小彩蛋： 全屏(✔)、 国际化语言切换（✔）、返回首页（✔） 

![gitdatav02](https://camo.githubusercontent.com/163ce7bfcdcd0cc9a0e1ac66b6e488fdbba1ad0e/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031392f372f392f313662643463643032333861343162613f773d3138393726683d39343026663d706e6726733d343934343737)

### DataVisualization

github地址： https://github.com/SimonZhangITer/DataVisualization 

> 将数据通过图表的形式展现出来将大大的提升可读性和阅读效率

> 本例包含柱状图、折线图、散点图、热力图、复杂柱状图、预览面板等

![dd](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/bi/demoBI.jpg)