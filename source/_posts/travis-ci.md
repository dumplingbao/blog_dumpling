---
title: 持续集成部署-Travis CI
date: 2019-08-22 10:01:17
categories: 
 - 持续集成
tags:
 - hexo
 - travis ci
---
## 概述

​	对于一个程序猿来说，软件开发是本职工作，你会经常听到”发布到测试服务器测试一下“、”今晚弄完打个包发布到正式环境上“，你会发现集成部署已然成为程序员的必备技能，更何况还有转岗专门做实施的。

​	持续集成实际就是解决这些中间环节，自动化完成，就像构建、测试、集成类的工作，程序猿只需要提交代码剩下的活就自动干了。

​	持续集成是一种软件开发实践，即团队开发成员经常集成他们的工作，通常每个成员每天至少集成一次，也就意味着每天可能会发生多次集成。每次集成都通过自动化的构建（包括编译，发布，自动化测试）来验证，从而尽早地发现集成错误。（来自百度百科）

<!--more-->

​	持续集成部署工具：

- Gitlab CI

- Drone CI

- Jenkins

- Travis CI

这里不去介绍各自特点，有兴趣可以自己研究一下，只介绍一下Travis CI，市场份额最大的一个，不过好多人说是因为Travis CI绑定github的原因，所以也是主要说Travis CI结合github实现持续集成部署。

## Travis CI

github建两个分支：一个放源代码，一个放编译后的文件

这里以hexo搭建博客为例

![blog-master](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Travis/blog-master.png)

## 设置GitHub_Token

先到github上添加token

![blog-githubtoken](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Travis/blog-githubtoken.png)

到Travis上添加token

![blog-token](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Travis/blog-token.png)

## 配置Travis CI

配置Travis CI实现自动编译发布

到Travis官网，进入设置，打开要发布的公开仓库

![blog-public](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Travis/blog-public.png)

创建` .travis.yml `文件

```
language: node_js
node_js: stable

cache:
  directories:
    - node_modules

branches:
  only:
    - hexo

before_install:
  - npm install -g hexo-cli
  

# S: Build Lifecycle
install:
  - npm install
  - npm install hexo-deployer-git --save


# before_script:
 # - npm install -g gulp
    
script:
  - hexo clean
  - hexo generate

after_script:
#  - cd ./public
#  - git init
  - git config user.name "gattia"
  - git config user.email "gattia.su@gmail.com"
#  - git add .
#  - git commit -m "Update docs"
#  - git push --force --quiet "${Travis}@${GH_REF}" master:master
  - sed -i "s/Travis/${Travis}/g" ./_config.yml
  - hexo deploy
```



注意部署路径，修改提交测试一下

发布完成

![blog-finish](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Travis/blog-finish.png)

可以查看发布历史

![blog-History](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Travis/blog-History.png)

点击项目右上角的build按钮，选择Markdown

![blog-mark](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Travis/blog-mark.png)

将其复制到github项目下README中，图标开通成功

![blog-pic](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/Travis/blog-pic.png)
参考链接：https://segmentfault.com/a/1190000016603414?utm_source=tag-newest