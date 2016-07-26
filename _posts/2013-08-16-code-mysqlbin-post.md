---
layout: post
title: 主从数据库
description: ""
modified: 2016-06-01T15:27:45-04:00
tags: [sample post, code, highlighting]
image:
  feature: abstract-10.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
---

### 读写分离
> 主数据库负责写，从库负责读
> 从库复制主库bin-log日志

  


### 如何查看bin-log
使用mysql提供命令  mysqlbinlog 查看bin-log内容


### bin-log的作用
1. 可以做数据恢复
    数据库经常做备份。

        16:11    做好一个备份  a.sql

        16:20   用户插入了一条新数据


        16:50   数据库数据发生丢失



        17:00  做数据恢复

             a.sql    


2. 可以做读写分离


### 主从配置
优点：1. 减轻数据库压力 2. 提高查询速度  
      3. 从库可以备份数据

缺点：1. 数据同步可能存在时间差。

对策：数据实时性要求高的场合，数据还是从主库查询。

建议开启从库的bin-log,原因：1. bin-log可以做备份  
  2. 万一主库挂掉，从库可以迅速转换成主库。



### 重点
    1. 会话控制 
        a. 为什么需要会话控制
        b. SESSION与COOKIE的区别与联系
        c. 如果禁用COOKIE，SESSION是否还可以使用，怎么识别用户？

    2. 各种区别
        a. post与get区别
            从协议角度看，post与get区别在于，get一般用来获取
            服务器的数据，post一般用于向服务器发送数据。

            从具体应用(表单)：
                1. 大小
                2. 安全性
                3. 

        c. include require区别  include_once

        d. echo ,print ,print_r 区别
            echo,print是语言结构，print_r是函数

    3. 数据库优化(*****)


    4. 数据库基本操作(****)

    5. 能够写出一些简单的正则
        匹配到a连接中的href里面的内容
        <link rel="stylesheet" type="text/css" href="fdsjakf">
        <a href="fdsajfk">fdsjak</a>

    6. 面向对象一些概念

    7. javascript

    8. 你会哪些版本控制器
        svn  git

<script src="https://gist.github.com/mmistakes/43a355923921d22cd993.js"></script>