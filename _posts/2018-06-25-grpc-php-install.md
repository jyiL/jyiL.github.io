---
layout: post
title: GRPC PHP Install
description: ""
tags: [GRPC, PHP]
image:
  background: triangular.png
---

## 安装GRPC php客户端所需扩展

* 自行安装PHP、PECL、Composer

#### 安装grpc扩展
	sudo pecl install grpc
	安装成功后添加php.ini
	extension=grpc.so


* `注意：`<a href="https://grpc.io/docs/quickstart/php.html#build-and-install-the-grpc-c-core-library" target="view_window">`源码安装请查看官网示例`</a>
* phpinfo()或php -m | grep grpc查看是否安装成功

#### 安装protobuf

<a href="https://github.com/google/protobuf/releases" target="view_window">`protobuf版本`</a>

	wget https://github.com/google/protobuf/releases/download/v3.6.0/protobuf-all-3.6.0.tar.gz
	tar zxvf protobuf-all-3.6.0.tar.gz
	cd protobuf-3.6.0
	./configure --prefix=/usr/local/protobuf
	make && make install
	export PATH=/usr/local/protobuf/bin:$PATH
	protoc --version    // 没有报错，显示版本号说明安装成功

#### 安装protobuf扩展
	sudo pecl install protobuf
	安装成功后添加php.ini
	extension=protobuf.so
* phpinfo()或php -m | grep protobuf查看是否安装成功

#### 构建gRPC PHP protoc插件
	git clone -b $(curl -L https://grpc.io/release) https://github.com/grpc/grpc
	cd grpc
	git submodule update --init
	make grpc_php_plugin

* `注意`运行git submodule update --init如果报下面错误，
* Peer reports incompatible or unsupported protocol version
* 执行yum update -y nss curl libcurl即可
* 在运行git submodule update --init期间可能会因网络问题中断，请重新运行并且耐心等待
