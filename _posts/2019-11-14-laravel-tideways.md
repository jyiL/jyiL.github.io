---
layout: post
title: Laravel Install Tideways
description: ""
tags: [Laravel, Tideways, PHP]
image:
  background: triangular.png
---

## laravel安装tideways

因为本人开发环境采用docker,所以这里也是采用docker安装.如果用源码安装请自行下载源码包编译php扩展

### 安装tideways扩展

```
    # 源码包地址
    wget https://github.com/tideways/php-xhprof-extension/archive/v4.1.6.tar.gz
    
    # docker构建 本人已打包好镜像,内含nginx+php-fpm 直接pull即可
    docker pull jianyl/nginx-php-fpm
    docker run -d -p 8080:80 -v /data/wwwroot:/var/www/html -v xxx:/etc/nginx/sites-enabled/ jianyl/nginx-php-fpm
```
`注意:` <a href="https://github.com/jyiL/docker/blob/master/Dockerfile-nginx-php-fpm#L14" target="view_window">`如需自行构建dockerfile安装tideways扩展请参照以下代码`</a>

### 安装mongo

```
    docker run -p 27018:27017 --name=tideways-db -v tideways-db:/data/db -d mongo
    
    docker exec -it tideways-db bash
    $ mongo
    > use xhprof
    > db.results.ensureIndex( { 'meta.SERVER.REQUEST_TIME' : -1 } )
    > db.results.ensureIndex( { 'profile.main().wt' : -1 } )
    > db.results.ensureIndex( { 'profile.main().mu' : -1 } )
    > db.results.ensureIndex( { 'profile.main().cpu' : -1 } )
    > db.results.ensureIndex( { 'meta.url' : 1 } )
```

### 安装xhgui

```
    // 进入刚刚运行的nginx+php-fpm容器
    docker exec -it xxx bash 
    cd /var/www/html
    git clone https://github.com/laynefyc/xhgui-branch.git
    cd xhgui-branch
    php install.php
    
    # 修改配置文件
    vi xhgui-branch/config/config.default.php
    
    # 这里需要特别注意,因为本人安装的是v4.1.6版本,如果安装的是v5版本需改为'tideways_xhprof'
    'extension' => 'tideways',
    
    'save.handler' => 'mongodb',
    
    'db.host' => 'mongodb://172.19.0.1:27018',
    'db.db' => 'xhprof',
```

### nginx配置

```
    # 项目nginx配置添加PHP_VALUE，告诉PHP程序在执行前要调用服务：
    location ~ \.php$ {
        # tideways 扩展
        fastcgi_param PHP_VALUE "auto_prepend_file=/var/www/xhgui-branch/external/header.php";
        ......
        ......
        ......
    }
    
    # xhgui前端展示页面配置 指向安装的xhgui的webroot目录
    server {
        listen   80; ## listen for ipv4; this line is default and implied
        
        root /var/www/xhgui-branch/webroot;
        index index.php index.html index.htm;
        server_name tideways.demo.com;
        
        sendfile off;
        
        error_log /dev/stdout info;
        access_log /dev/stdout;
        
        location / {
            index  index.php;
            if (!-e $request_filename) {
                rewrite . /index.php last;
            }
        }
        
        location ~ \.php$ {
            try_files $uri =404;
            fastcgi_split_path_info ^(.+\.php)(/.+)$;
            fastcgi_pass unix:/var/run/php-fpm.sock;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            fastcgi_param SCRIPT_NAME $fastcgi_script_name;
            fastcgi_index index.php;
            include fastcgi_params;
        }
    }
```

此时打开浏览器输入tideways.demo.com:8080应该能看到相应的效果
