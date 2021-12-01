---
title: 我的2021年新Mac设置
date: 2021-11-27 23:48:58
categories: 
 - [MacBook]
 - [Mac]
tags:
 - MacBook
---

看到[SHAWN **@SWYX** WANG](https://www.swyx.io/)的`My 2021 New Mac Setup`的文章，相同的是都是用Mac作为程序开发，因此借着我的新Mac，我也进行了整理，写下这一份整理笔记。

<!--more-->
# 系统设置

这个根据个人不同的喜好

- 修复触控板方向：触控板 -> 滚动和缩放 - 自然关闭
- 禁用 Spotlight 搜索除应用程序和系统首选项之外的所有杂项
- 禁用询问 Siri
- 禁用字典查找：触控板 -> 指向和点击 -> 查找和数据检测器关闭
- 首选项 → 显示文件扩展名
- 从 程序坞（Dock） 中删除所有内容，除了：Finder、系统偏好设置和垃圾箱
- 启用三指拖移：辅助功能 -> 指针控制 -> 启用拖移。（备注：启用后原三指操作变为四指）



# 浏览器设置

Chrome自然必不可少，下载安装即可，设置为默认

**扩展程序**：

- [Morpheon 黑暗主题](https://chrome.google.com/webstore/detail/morpheon-dark/mafbdhjdkjnoafhfelkjpchpaepjknad?hl=en)

- [React Devtools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)

- [Vue.js devtools](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=zh-CN)

- [GitHub-Chart](https://chrome.google.com/webstore/detail/github-chart/apaldppjjcjgjddfobajdclccgkbkkje?hl=zh-CN)

- [go get for Github](https://chrome.google.com/webstore/detail/go-get-for-github/ahkfiobnoafagbaaghmbbopfdpdbaidi?hl=zh-CN)

- [Markdown Nice](https://chrome.google.com/webstore/detail/markdown-nice/blndbjkicjhcbpldeamfbdoeekcbampi?hl=zh-CN)

- [Octotree - GitHub code tree](https://chrome.google.com/webstore/detail/octotree-github-code-tree/bkhaagjahfmjljalopjnoealnfndnagc?hl=zh-CN)

- [Sourcegraph](https://chrome.google.com/webstore/detail/sourcegraph/dgjhfomjieaadpoljlnidmbgkdffpack?hl=zh-CN)

  

# 终端设置

命令行真的是程序员必备的了，这个终端就很重要了，Mac的终端默认是bash，这个可真是太bash了，连`ll`命令都不行，当然可以配置。还是不推荐bash（就像window下的cmd，宁愿选择git下的bash），这个确实没法让我们起飞。

备注：bash就是shell一种增强版本，就是经常在sh文件里看到的`#! /bin/bash`

shell有很多种，常用的基本上就是`bash`、`zsh`、`fish`了，他们的区别无外乎就是命令、格式（包括高亮、特殊格式显示）、提示信息等方面上的区分。

可以先看一下Mac提供了哪些shell（可以直接到bin目录下看），可以通过偏好设置就像修改，也可以直接通过下面的命令设置，设置完成需要重新进入终端

```
# 查看所有的shell
cat /etc/shells
# 查看系统用户默认shell
cat /etc/passwd | grep sh
## 系统默认的终端为bash，切换该终端为zsh
chsh -s /bin/zsh
# 切回默认终端bash
chsh -s /bin/bash
# 切换终端fish
chsh -s /bin/fish
```

没有需要安装，这里我安装的zsh而且是[Oh My Zsh](https://ohmyz.sh/#install)，安装简单，直接看官网。

Oh My Zsh可以设置主题和插件，提示信息非常nice，详细配置不再赘述，我摘了一段和zsh的区别：

```
#  Zsh 是一个shell，就像bash或fish 一样，它解释命令并运行它们。Oh My Zsh 是一个构建在 zsh 之上的框架，其结构允许它拥有插件和主题，并从一开始就提供我们认为最好的设置。您可以在没有 Oh My Zsh 的情况下使用 zsh，但如果您没有 zsh，则无法使用 Oh My Zsh。
```

如果需要fish的，推荐阮一峰的文章：[Fish shell 入门教程](http://www.ruanyifeng.com/blog/2017/05/fish_shell.html)

![terminal](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/2021/mac/terminal.png)

## 超级终端（[Hyper Terminal](https://hyper.is/)）

[Hyper Terminal](https://hyper.is/)，一个通用的串行交互软件，可以通过串口、调制解调器或以太网连接，使用该程序连接到其他计算机、Telnet 站点、公告板系统 （BBS）、联机服务和主机、嵌入式系统等。

超级终端还能对外观进行主题设置并可以安装插件，配置直接通过修改`.hyper.js`即可。

![hyper](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/2021/mac/hyperpower.gif)

# 环境及应用程序安装

## 关闭SIP

关闭SIP（SIP 全称为「System Integrity Protection」即「系统完整性保护」）还是有必要的，关闭之后才能修改某些文件的权限，一般建议修改完再打开，长期关闭回更方便，即使有些软件sudo进行安装也可以，还是关闭了吧。

以前的快捷键Command+R进入到恢复模式，M1芯片直接按住电源按钮即可进入。

顶部菜单拉中打开终端，输入：

```
csrutil disable
```

然后重启，设置权限就可以了

```
sudo chmod -R 777 目标文件夹
或者
sudo chmod a+w 目标文件夹
```

## Homebrew

安装：

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

如果443，修改host文件

```undefined
sudo vi /etc/hosts
# 加入
199.232.4.133 raw.githubusercontent.com
```

或者使用国内源

使用国内源：

```
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

配置的中科大的资源

## jdk

官网下载安装（1.8），默认不需要配置环境变量

## maven

官网下载最新版，解压配置环境变量即可

```undefined
 // vi ~/.bash_profile

export MAVEN_HOME=/Users/bao/software/apache-maven-3.8.4
export PATH=$PATH:$MAVEN_HOME/bin
```

## git

```
brew install git
git --version
git config --global user.name "用户名"
git config --global user.email "邮箱"
```

## nodejs

直接安装

```
 ~/ brew install node
 ~/ node -v
```

但是实际工作会涉及到多node版本的问题，建议还是用`n`或者`mvn`来管理，这里我们用`n`，个人感觉好用轻巧

```
 ~/ brew install n
 ~/ n stable     // 安装最新稳定版
 ~/ n 14.17.0
// 历史版本地址：https://nodejs.org/zh-cn/download/releases/
 ~/ n ls         // 列出所有版本
node/14.17.0
node/16.13.0
 ~/ n            // 切换版本
 ~/ n rm <version>  // 删除某个版本
```

如果已经安装了，node卸载方法

验证

```text
 ~/ which node
/User/<your's-user-name>/.nvm/versions/node/<latest-node-lts-version>/bin/node
```

卸载

```text
 ~/ rm -rf /usr/local/{bin/{node,npm},lib/node_modules/npm,lib/node,share/man/*/node.*}
```

## IntelliJ IDEA （v2021.2.3）

官网下载，需破解

![idea](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/2021/mac/idea.png)

## VSCode

官网下载即可，简单

## 其它

- Redis Desktop Manager（需破解）
- Navicat Premium 15（需破解）
- UltraEdit（需破解）
- FinalShell

![desk](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/2021/mac/deskbook.gif)

# 其它应用

- CleanMyMac X （电脑清理、破解版）
- WPS Office
- 超级右键
- iShot（截图工具）
- Typora（Markdown编辑工具）
- AppDelete（软件卸载、破解版）
- Photoshop
- Parallels Desktop（Linux、win11、破解版）
- MindNode（思维导图）

# 最后一游

Wonderbox

![Wonderbox](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/2021/mac/Wonderbox.jpg)

