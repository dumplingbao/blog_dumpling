---
title: Metabase-BI系列03：win系统用git-bash启动Metabase
date: 2019-11-08 09:37:34
categories: 
 - [Metabase]
 - [可视化工具]
tags:
 - BI
 - Metabase
---
由于windows下启动Metabase存在不友好情况，启动linux消耗系统性能，影响工作效率，巧妙借助git-bash来启动Metabase，既可以避免windows下的坑，还特别的轻量级，甚至比cmd、vscode等命令窗口下启动都要快。
<!--more-->
## 概述

git目前最好用的版本控制器了，github、gitlib、coding、码云等代码托管平台，都使用的git，而且git本身就是用于linux内核开发的版本控制工具。所以大部分开发者本身就用，只要装了git，就带有git-bash，就在git的安装目录下面。

![git-bash01](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/bash/git-bash01.jpg)

windows下cmd命令的文件格式：`E:\Profile Files`，`git-bash`下：`/e/Profile Files`，是不是很像linux。

![git-bash02](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/bash/git-bash02.jpg)

## git-bash启动Metabase

其实很简单，就是进到Metabase目录下面启动，唯一需要注意的就是leiningen，leiningen是Metabase的项目构建和项目启动的组件，windows下安装配置是bat命令，所以直接在git-bash下，直接启动是找不到lein命令的。

在`Metabase-BI系列01：二次开发环境(windows)搭建`下提到过`leiningen`的安装，直接福复制的命令，创建了个`bat`文件，这里你就需要下载官网的[lein script](https://leiningen.org/#install)，直接网页搜索`lein script`就能快速找到，这个是linux的lein的脚本命令，不需要扩展名，名称lein就可以，把脚本命令复制`lein安装目录/bin`进去，其他步骤一样，同样需要`leiningen-2.9.1-standalone.jar`。

如果是用的zip包安装的，本身`bin`下面就有了lein这个linux脚本命令，只需要下载或者直接打包`leiningen-core-2.9.1.jar`，放到`安装目录/self-installs`下，缺少`jar`包会报缺少依赖错误。

别忘了windows下面配置环境变量，如果需要在`git-bah`下配置环境变量应该也可以，环境变量位置：`git安装目录/etc/profile`，配置完需执行命令，使其生效

```
export LEIN_HOME=/e/metabase/lein/leiningen-2.9.1
export PATH=$LEIN_HOME/bin:$PATH
```

```
source /安装目录/etc/profile
```

可以通过如下命令，查看环境变量是否生效

```
echo %LEIN_HOEM%  //windows下
echo $LEIN_HOME   //git-bash下
```

![git-bash04](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/bash/git-bash04.jpg)

安装完启动`lien run`

![git-bash03](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/bash/git-bash03.jpg)



## git-bash打包Metabase驱动包

`Metabase`目录有个`module/dirviers`模块，下面放着`mongo`、`oracle`、`sparksql`等数据库驱动项目，可以直接到各个驱动下面执行`lein uberjar`

如果在`git-bash`下，我们就可以到`Metabase`安装目录下，执行命令，注意一定是安装目录下而不是安装目录`bin`下面

```
#Metabase的doc下有说明
# Build the 'mongo' driver 单独执行一个驱动
./bin/build-driver.sh mongo
# Build all drivers 打包所有驱动
./bin/build-drivers.sh
```

## git-bash打包转换语言包

`Metabase安装目录/bin/build-translation-frontend-resource`文件，进行语言包po到json的转换

Metabase语言包（po）位置：`Metabase安装目录/local`

用法：

```
./bin/i18n/build-translation-frontend-resource input.po output.json
```

## git-bash缺点

git-bash目前发现唯一的一个缺点就是，控制台日志查找不方便，不如vscode直接在控制台`ctr+f`就能查找定位。