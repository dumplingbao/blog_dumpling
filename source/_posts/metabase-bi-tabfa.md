---
title: Metabase-BI系列10：新版透视表实现方案
date: 2020-02-27 22:40:00
categories: 
 - [Metabase]
 - [可视化工具]
tags:
 - BI
 - Metabase
---

​	之前已经介绍了复杂表头表格的开源PR，时间较早，基于Metabase V0.30版本的实现，而Metabase V0.33版本之后变化较大，所以如果在新版上实现PR中Metabase 复杂表头表格功能需要做改造。当然如果实力允许，你也可以按照自己的思路去实现。

​	详情可以到github上看一下,PR：[Metabase pull request #8427](https://github.com/metabase/metabase/pull/8427)

<!--more-->
​	效果如下：

​	![dt](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/newtab/table_dt.gif)

## 实现

​	准备工作：到r-hot下面找到fork下来的Metabase下面，找到summaryTable这个分支，clone一份。

​	由于Metabase 架构完善，高度组件化，所以不用担心对代码的影响，耦合性相对很低。

​	作者就是新加了一种可视化图表的类型：

​	![tableksh](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/newtab/table_ksh.jpg)

​	所以，可以顺着这个新增的可视化组件，找到需要的文件，透视表所用到的组件大部分以summary开头或者包含summary，这里就不一一列出了，然后放到最新版的Metabase源码对应目录下面，然后编译启动就可以了。

​	注意：Metabase透视表功能用到了一些开源的npm组件，需要引入，也可以看我上一篇文章实现解析里面提到的内容

​	但是由于Metabase新旧版本返回的数据结构有所变化所以需要处理之后才能实现正常的功能，所以给出了下面的两种解决方案：

![summary04](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/newtab/summary04.jpg)

## 方案一

​	Metabase为什么把columns去掉不得知，也不像是因为用不到就就去掉的，起码在我看来保留这么一个设计还是很方便的，透视表PR就充分利用了这一点。新版的虽然说是去掉了，但是也不是完全去掉，只是屏蔽掉了。所以方案一就是把columns放出来就可以了，透视表的功能就能正常使用。

找到`代码目录/src/metabase/query_processor/middleware/dev.clj`中的代码

```
  ;; 放开columns权限，将columns换个名字就可以了，这里我加了AA
  (complement :columnsAA)
  "QP results should no longer include :columns."))
```



## 方案二

​	如果考虑官方既然已经屏蔽了，考虑以后代码更新，你不想放开colums，那就需要在透视表用的代码里面对columns进行处理，其中一种方式就是cols里面获取，当然你也可以用其他方式处理甚至是替换columns处理的方式，推荐一种处理方式如下：

```
  columns = []
  if(cols.length>0){
​    cols.map(item=>{
​      columns.push(item.name)
​    })
  }
```

 	备注：里面用到columns的地方挺多，需要处理的地方挺多，目前不确定会不会带来其它问题，但是就目前来看这种处理方式效率会低，本来这种js实现的透视表效率就是一直讨论的问题，所以我建议还是采用方案一的方式效果更好。
