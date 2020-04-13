---
title: Metabase-BI系列12：0.35版本表达式是真的香
date: 2020-04-12 00:00:00
categories: 
 - [Metabase]
 - [可视化工具]
tags:
 - BI
 - Metabase
---

​	2020年3月30日，Metabase0.35版本真的来了，正如官方所说，0.35真的是一个重要的版本，最重磅的升级就是自定义表达式的升级，直接使Metabase的查询编辑器有了质的飞跃，对数据分析中自定义列、数据过滤器、数据聚合功能有了很大的提升。

<!--more-->
​	这个重要的升级通俗讲：

1. 提升数据处理能力，不用写sql就能进行更多的数据转换
2. 不同数据库，通用处理规则，提升用户体验
3. 改为用`[]`方式获取字段，自动提示操作十分人性化

## 升级前后对比

| 序号 | 功能     | 升级前                             | 升级后（0.35之后） |
| ---- | -------- | ---------------------------------- | ------------------ |
| 1    | 自定义列 | 只支持数字类型的`+`，`-`，`*`，`/` | 1、支持字符字段<br>2、新增字符和数字处理18个函数`abs`，`concat`等<br/>3、支持升级前功能 |
| 2    | 过滤器 | 1、单个字段判断筛选<br>2、数字类型`是`，`不是`，`为空`，`不为空`<br>3、字符类型除上面四个外，还有`包含`，`不包含`，`以..开始`，`以..结束`<br>4、其它类型不再赘述，升级前后未变化 | 1、支持自定义表达式<br>2、新增函数支持`between`，`contains`，`ednWith`，`startWith`，`interval`<br>3、扩展运算符：`AND`，`OR`，`NOT`，`>`，`>=`，`<`，`<=`，`=`，`!=`<br>4、支持升级前功能 |
| 3    | 聚合     | 1、支持自定义表达式<br>2、原有聚合函数的`+`，`-`，`*`，`/` | 1、支持自定义表达式<br>2、新增了9中数字的处理函数<br>3、支持升级前功能 |

过滤器：

![filter-expressions](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/12/filter-expressions.gif)

聚合：

![first-expression](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/12/first-expression.png)

## 新增表达式介绍

这次表达式确实扩展了很多，这里挑了几个常用的预览一下，也可以去Metabase官方查看全部表达式

| 名称           | 句法                                          | 备注                                                    | 例                                                           |
| -------------- | --------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------ |
| Between        | between(column, start, end)                   | 1、日期范围内     2、数字区间内                         | between( [Rating], 3.75, 5 )                                 |
| Case           | case(condition, output,  condition, output …) | 多情况判断，如枚举类型                                  | case( [Weight] > 200,  "Large", [Weight] > 150, "Medium", "Small" ) |
| 返回非空值     | coalesce( value1, value2, …)                  | 按顺序查看每个参数中的值，并为每行返回第一个非空值。    | coalesce( [Comments], [Notes],  "No comments" ）             |
| 字符串拼接     | concat(value1, value2, …)                     | 1、字符串或字段拼接     2、数值会转换字符拼接（已测试） | concat([Last Name] , ",  ", [First Name])                    |
| 长度           | length(text)                                  | 返回文本中的字符数                                      | length([Comment])                                            |
| 小写转换       | lower(text)                                   | 以小写形式返回文本字符串。                              | lower( [Status] )                                            |
| 去除左空格     | ltrim(text)                                   |                                                         | ltrim( [Comment] )                                           |
| 正则表达式提取 | regexextract(text,  regular_expression)       | 根据正则表达式提取匹配的子字符串                        | regexextract( [Address],  "[0-9]+" )                         |
| 替换           | replace(text, position, length,  new_text)    |                                                         | replace( [Order ID], 8, 3,  [Updated Part of ID] )           |
| 去除右空格     | rtrim(text)                                   |                                                         | rtrim( [Comment] )                                           |
| 截取字符串     | substring(text, position,  length)            |                                                         | substring( [Title], 0, 10 )                                  |
| 去除空格       | trim(string)                                  |                                                         | trim( [Comment] )                                            |
| 大写转换       | upper(text)                                   |                                                         | upper( [Status] )                                            |

也可直接查看更多表达式连接（谷歌自动翻译）：https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Metabase/12/Metabase-bds.html

## 0.35其它说明

- 新增表达式并不能兼容所有的数据库，因为数据库差异很大，做到这种程度已经很牛x了，表达式数据库兼容情况参加官方说明
- 此外0.35版本调整了查询结果缓存内存的策略，改为数据库流，调整了一些api，着眼性能提升
- 当然少不了的就是修复bug了，参加官方说明

## 交流学习

刚建的群，学习Metabase、Davinci等开源BI，群号：72569367，感兴趣的可以加一下。

