---
layout: post
title: Swoole Chat
description: ""
tags: [Swoole, PHP]
image:
  background: triangular.png
---

## swoole实现的一个简单聊天室功能

今天心血来潮花了点时间查看了一下swoole官方文档，写了一个websocket服务
思路大概就是利用swoole的广播从而实现一个聊天室功能

### 关键代码

```
     // $server->connections 遍历所有websocket连接用户的fd，给所有用户推送
    foreach ($ws->connections as $fd) {
        // 需要先判断是否是正确的websocket连接，否则有可能会push失败
        if ($ws->isEstablished($fd)) {
            $ws->push($fd, json_encode($msg, JSON_UNESCAPED_UNICODE));
        }
    }
```

主要就是监听message，当有客户端发送消息后，
通过connections获取所有的websocket用户，然后foreach循环推送新消息给所有客户端，达到广播效果
然后利用swoole table存储用户的昵称，在广播时将用户昵称一并返回给客户端

### 效果图
<img srcset="data:image/gif;base64,R0lGODdhAQABAPAAAMPDwwAAACwAAAAAAQABAAACAkQBADs=" data-src="{{ site.url }}/images/swoole/1583828934055.jpg" class="lazyload" />


### <a href="https://github.com/jyiL/swoole-chat" target="view_window">`项目地址`</a>