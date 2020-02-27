---
title: Metabase-BI系列02：二次开发环境(windows)踩坑
date: 2019-11-06 23:50:29
categories: 
 - [Metabase]
 - [可视化工具]
tags:
 - BI
 - Metabase
---
由于github源代码开发者用的linux或者mac来作为开发环境，所以存在windows环境下的不友好，有的甚至就是坑。甚至好多Metabase windows开发者也是用linux虚拟机来跑环境的。

当然如果通过虚拟机跑linux，或者用windows自带的Ubuntu进行开发，有些问题就不存在了，还有一种好的方式用git的bash环境去搭建，可参见：`Metabase-BI系列03：win系统用git-bash启动Metabase`。
<!--more-->
## 坑一：git-clone源码编码LF问题

git里面有 **AutoCRLF** 、 **SafeCRLF** 

```
git config --global core.autocrlf true   //提交转换为LF，检出转换为CRLF 
git config --global core.autocrlf false  //提交检出都不转换 
git config --global core.autocrlf input  //提交转换为LF，检出不转换 
git config --global core.safecrlf true   //拒绝提交包含混合换行符文件 
git config --global core.safecrlf false  //允许提交包含混合换行符文件 
git config --global core.safecrlf warn   //提交包含混合换行符文件给出警告 
```

如果： **AutoCRLF** 设置为ture，代码转换成CRLF，代码后台能跑起来，前台web端访问空白，js错误，各种property undefined

![web](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/keng/report-web.jpg)

所以这种情况一定要将**AutoCRLF** 设置为false，而且是先设置false，再执行git clone

可以肯定的一点是，本身LF转CRLF就存在bug，所以就很容易出问题

## 坑二：yarn install时grep命令问题

在install的时候，要去掉grep那条命令，此命令为linux命令，可见完全没有考虑windows环境的感受，当然你也可以尝试安装grep在windows下执行的程序

```
"preinstall": "echo $npm_execpath | grep -q yarn || echo '\\033[0;33mSorry, npm is not supported. Please use Yarn (https://yarnpkg.com/).\\033[0m'",
```

## 坑三：NODE_ENV不是内部或外部命令

在install的时候，NODE_ENV不是内部或者外部命令，其实这个还是linux和windows的问题

```
linux： NODE_ENV=hot 
windows: set NODE_ENV=hot
```

所以说windows下，直接加个set就可以了，或者去掉里面的，先set变量，再install。

注意：这个是热部署的变量，如果需要热部署，这个命令不能少。

![node_env](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/pit/node_env.png)


## 坑四：lein ring server文件名扩展名报错

```
Compilation failed: Cannot run program "java" (in directory "D:\metabase"): CreateProcess error=206, 文件名或扩展名太长。
Error encountered performing task 'ring' with profile(s): 'base,system,user,provided,dev,ring'
Compilation failed: Cannot run program "java" (in directory "D:\metabase"): CreateProcess error=206, 文件名或扩展名太长。
```

这个问题是windows系统导致的，如果是win10或许可以解除这个长度限制，如果是win7或者更早的windows版本估计不好解决，就不能用热部署了，但是lein run是可以的

## 坑五：自动创建仪表板(数据透视)的问题

登录首页后，一直发请求，html 500错误

```
Request URL:  http://localhost:3000/api/automagic-dashboards/database/2/candidates
Request Method: GET
Status Code:  500 Server Error 
```

![automagic](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/keng/metabase-automagic01.jpg)

后台报错，找不到目录文件

![automagic02](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/keng/metabase-automagic02.jpg)

很明显是windows路径错误，可以特殊处理一下，找到src/metabase/util/files.clj

```
(defn do-with-open-path-to-resource
 "Impl for `with-open-path-to-resource`."
 [^String resource, f]
 (let [url (io/resource resource)]
  (when-not url
   (throw (FileNotFoundException. (trs "Resource does not exist."))))
  (if (url-inside-jar? url)
   (with-open [fs (jar-file-system-from-url url)]
​    (f (get-path-in-filesystem fs "/" resource)))
   ; windows下修复数据透视路径subs去掉前缀
   (f (get-path (subs (.getPath url) 1))))))
```



## 坑六：数据源、中文汉化包不自动生成问题

正常情况下会在，安装目录/plugins下生成数据库驱动包，如果没有新增数据库只能选择默认的几种数据库类型

![driver](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/keng/Metabase-driver.jpg)

正常情况下，安装目录/resources/frontend_client/app/locales下，生成多语言版本json

![language](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/keng/language.jpg)

windows下有些shell脚本命令是不能正常运行的，所以导致不能正常生成，可以直接下载放到相应目录底下，数据库驱动包也可以到module各个驱动目录下面直接lein uberjar生成，这种生成容易发生依赖包不一致报错的问题。

## 问题七：Metabase多版本jar包冲突

Metabase里面有很多分支，有些是版本分支，如果用到新老板，就不要把不同版交叉使用，Metabase在运行时正常会检测并清理，一旦出现检查存在又不进行清除更新就会出现jar包冲突。