---
title: 小程序：mqtt+webview控制显示内容
date: 2021-07-22 23:48:58
categories: 
 - [小程序]
 - [微信]
tags:
 - 小程序
---

看到mqtt+webview似乎不知道能做什么，mqtt微消息服务更适用iot物联网，这个应该熟悉，但是似乎还是得从webview说起。webview的场景不仅仅是手机端的APP或者小程序用到，好多基于android主板显示的设备、大屏等webview都发挥了很大的作用。这里我们一是验证小程序的mqtt，二是通过mqtt控制设备自动切换显示内容，这样试想一下，其实就是远程操控设备显示内容的一种很好的方式。

<!--more-->

## 效果

![01](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/2021/wx/mq/mqtt.gif)

## mqtt小程序端

需要mqtt.js客户端库：https://github.com/mqttjs/MQTT.js

小程序配置：

```
const app = getApp()
var mqtt = require('../../../utils/mqtt.min')
var client = null

Page({

  /**
   * 页面的初始数据
   */
  data: {
    webUrl: 'https://www.baidu.com'
  },

  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {
    this.connnectMqtt()
  },

  connnectMqtt: function (){
    var that = this
    const options = {
      connectTimeout: 4000, // 超时时间
      clientId: 'mqtt_' + Math.random().toString(16).substr(2, 8),
      port: 8083,  //重点注意这个坑
    }

    client = mqtt.connect("wx://xx.xx.xx.xx/mqtt", options);
    client.on('reconnect', (error) => {
        console.log('正在重连:', error)
    })

    client.on('error', (error) => {
        console.log('连接失败:', error)
    })
    client.on('connect', (e) => {
        console.log('成功连接服务器')
  　　　　　　　//订阅一个主题
        client.subscribe('test', {
            qos: 0
        }, function(err) {
            if (!err) {
                console.log("订阅成功")
            }
        })
    })
    client.on('message', function (topic, message) {
        console.log('received msg:' + message.toString());
        that.setData({
          webUrl: message.toString()
        })
        console.log(that.data.webUrl)
    })
  } 
})
```

```
<web-view src="{{webUrl}}" bindmessage="getmessage"></web-view>
```

## mqtt服务端安装

*EMQ X* 是一款完全开源，高度可伸缩，高可用的分布式 MQTT 消息服务器

git地址：https://gitee.com/emqx/emqx

### docker安装步骤

```
$ docker search emqx // 查看版本
```

```
$ docker pull emqx/emqx // 拉取镜像
```

```
$ docker run -dit --name emqx -p 18083:18083 -p 1883:1883 -p 8083:8083 -p 8084:8084 emqx/emqx:latest // 运行
```

```
$ docker exec -it  emqx /bin/sh // 进入命名
```

### web管理界面

`http://127.0.0.1:18083`

`#账号： admin`

`#密码: public`

### 端口介绍

`1883：MQTT 协议端口`

`8883：MQTT/SSL 端口`

`8083：MQTT/WebSocket 端口`

`8080：HTTP API 端口`

`18083：Dashboard 管理控制台端口`

## mqtt客户端工具

我们没必要写后台代码，直接用个mqtt客户端工具做测试，用的MQTTX，这个就根据个人习惯选了

MQTTX地址：https://github.com/emqx/MQTTX/releases

安装完成配置验证即可。

## 代码

![diss](https://ossbao.oss-cn-qingdao.aliyuncs.com/blog/2021/wx/ssn/diss.jpg)

已提交github，扫码关注公众号（diss带码），回复：webview，获得源码github地址


