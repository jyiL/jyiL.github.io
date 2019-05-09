---
layout: post
title: Laravel Aliwaremq
description: ""
tags: [Laravel, PHP, RabbitMQ]
image:
  background: triangular.png
---

## 概述

这个扩展包是针对<a href="https://www.aliyun.com/product/amqp?spm=5176.234368.1278132.8.1743db25brEOTe" target="view_window">`阿里云消息队列AMQP（RabbitMQ）`</a>。通过简单的方法调用发送消费消息和接收消费消息。支持所有PHP的项目，本文针对Laravel作详细说明

#### 安装

    composer require jyil/aliwaremq

#### 发布配置文件
    php artisan vendor:publish --provider="Jyil\AliwareMQ\Laravel\Providers\LaravelServiceProvider" --force

`注意：Laravel5.5- 自行添加providers`

    Jyil\AliwareMQ\Laravel\Providers\LaravelServiceProvider::class

#### 配置

Key|描述
-----|-----
host|接入点
port|端口
virtualHost|资源隔离
accessKey|阿里云的accessKey
accessSecret|阿里云的accessSecret
resourceOwnerId|主账号id

#### 生产者

    app('aliwaremq')->send('queue', 'Hello World');

#### 消费者

    app('aliwaremq')->receive('queue');

#### 属性配置
    
    app('aliwaremq')->passive = false;
    app('aliwaremq')->durable = true;
    app('aliwaremq')->exclusive = false;
    app('aliwaremq')->autoDelete = false;
    app('aliwaremq')->noLocal = false;
    app('aliwaremq')->noAck = false;
    app('aliwaremq')->nowait = false;
    
* 使用本扩展包前请对<a href="https://www.rabbitmq.com" target="view_window">`RabbitMQ`</a>有一定的了解    