---
title: 安装 Swoole
date: 2020-04-12 20:23:57
tags: [环境 , Swoole]
description: laravel 官方推荐的环境是Homestead，就在Homestead环境下安装Swoole
---
以下以 `laravel` 官方推荐 `Homestead` 环境来举例

根据 `Swoole` 文档，直接 `install`

```
pecl install swoole
```

默认是按照最新版的，如果有你的环境 `php` 不是最新，那需要按照 `PHP` 版本去安装

下面是安装 `PHP` 是 7.2 版本的 swoole 的示例：

```
pecl -d php_suffix=7.2 install swoole
```

  - 也可指定版本：
 
```
pecl -d php_suffix=7.2 install swoole-1.9.23
```

如果报错，则试下 root 权限

```
sudo pecl -d php_suffix=7.2 install swoole
```

正常情况下这会就已经安装好了，修改下配置文件就可以了。但是我这一直报` ERROR: phpize faild `

我查找了好长时间才发现Homestead环境里的php默认没有`phpize` 

接下来只能重新安装php了，这是为了让phpize出来！因为不知道为什么现在homestead自带的没有

```
sudo apt install php7.2-dev
```

然后可能会下载不到资源，因为资源本身在国外。你需要修改下载源为国内的镜像源

apt下载源的配置文件是

```
 /etc/apt/sources.list
```

为保险起见，修改文件前，先备份一下文件

```
cp /etc/apt/sources.list  /etc/apt/sources_bak.list
```

然后修改文件

```
sudo vim /etc/apt/sources.list
```

将文件内容替换为（以清华源ipv6镜像站为例，其他国内镜像站类似）

```
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse 

# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse

deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse 

# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse

deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse 

# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse

deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse 

# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse

```

然后更新一下源

```$xslt
sudo apt-get update
```

安装的时候遇到提示都选择替换。

安装完毕检查/usr/bin会发现这时phpize和它对应的版本。

紧接着在执行安装`swoole`就可以了

```$xslt
sudo pecl -d php_suffix=7.2 install swoole
```

查找下 `php.ini` 的位置

```$xslt
php -i |grep php.ini
```
然后在 `php.ini` 中增加, `cli`和`fpm` 都要修改下

```
extension=swoole.s
```

记得重启下`nginx`

```$xslt
/etc/init.d/nginx restart
```

查看下是否已经成功加载了 `swoole`

```$xslt
php -m | grep swoole
```