---
layout: post
title: Tesseract OCR for PHP
description: ""
tags: [Tesseract OCR, PHP]
image:
  background: triangular.png
---

### Tesseract OCR for PHP

Tesseract是开源的OCR引擎。Tesseract最初设计用于英文识别，经过改进引擎和训练系统，它能够处理其它语言和UTF-8字符。Tesseract 3.0能够处理任何Unicode字符，但并非在所有语言上都工作得很好。Tesseract在庞大字符集语言（比如中文）上较慢，但是工作良好。

Tesseract需要知道相同字符的不同形状，也就是不同字体。最多允许的字体数量在intproto.h中通过MAX_NUM_CONFIGS定义，目前支持64种。当训练字体数量超过32种时，速度显著下降。

* github的一个Tesseract OCR开源库
> <a href="https://github.com/thiagoalessio/tesseract-ocr-for-php" target="view_window">`tesseract-ocr-for-php`</a>


## Installation

#### 1.安装依赖
* autoconf
* automake
* libtool
* libjpeg
* libpng
* libtiff
* zlib
* libjpeg-devel 
* libpng-devel
* libtiff-devel 
* zlib-devel


#### 2.安装Leptonica
><a href="http://www.leptonica.org/download.html" target="view_window">`Leptonica_download`</a>
    
	wget http://www.leptonica.org/source/leptonica-1.74.4.tar.gz
	tar xzvf leptonica-1.74.4.tar.gz
	./configure
	make && make install
	ldconfig

#### 3.安装Tesseract
	wget https://github.com/tesseract-ocr/tesseract/archive/3.05.00.tar.gz
	tar xzvf 3.05.00.tar.gz
	./autogen.sh
	./configure
	make && make install
	ldconfig

#### 4.安装Tesseract-OCR
	wget https://github.com/tesseract-ocr/tessdata/archive/3.04.00.tar.gz
	tar xzvf 3.04.00.tar.gz
	cp -r tessdata-3.04.00/* /usr/local/share/tessdata

#### 测试
	tesseract xxx.png out
	cat out.txt

## 在PHP上运行
	composer require thiagoalessio/tesseract_ocr
	php index.php

index.php
	
	<?php
		require_once './vendor/autoload.php';

		use thiagoalessio\TesseractOCR\TesseractOCR;
		echo (new TesseractOCR('./text.png'))
    	->run();
>注意：需编辑php.ini开启exec
