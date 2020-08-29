---
layout: 修改php-fpm和nginx运行用户
title: 修改php-fpm和nginx运行用户
date: 2020-04-09 11:57:20
categories: linux #分类
tags: [php, nginx, linux]
---

`nginx`和`php-fpm`是`www-data`用户运行,想要修改为 `www` 用户运行


## 修改nginx

```code
cd /etc/nginx
sudo vim nginx.conf
# 头部是这样
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

# 修改为
user www;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

# 重启nginx
sudo service nginx restart


```


## 修改php-fpm 

```$xslt
cd /etc/php/7.2/fpm/pool.d
sudo vi www.conf
# 找到
user = www-data
group = www-data

listen.owner = www-data
listen.group = www-data

# 都改为改为
user = www
group = www

listen.owner = www
listen.group = www

cd /run/php/
ls -al
# 这个目录下面有两个文件
# php7.2-fpm.pid和php7.2-fpm.sock
# 修改这两个文件的权限
sudo chown www:www php7.2-fpm.pid
sudo chown www:www php7.2-fpm.sock

# 重启php-fpm
sudo service php7.2-fpm restart
```