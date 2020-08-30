---
title: mac安装Homestead
date: 2020-08-30 10:36:35
tags: [环境] 
categories: Homestead  #分类
---
Laravel 致力于让整个 PHP 开发体验变的更愉快，包括你的本地开发环境。 Vagrant 提供了一种简单、优雅的方式来管理和配置虚拟主机。

## 安装与设置
### 1.安装VirtualBox 6.x
下载[VirtualBox](https://download.virtualbox.org/virtualbox/6.1.12/VirtualBox-6.1.12-139181-OSX.dmg)
### 2.安装Vagrant
下载[Vagrant](https://releases.hashicorp.com/vagrant/2.2.10/vagrant_2.2.10_x86_64.dmg)
### 3.安装 Homestead Vagrant Box
一旦将 VirtualBox 和 Vagrant 都安装好之后，你可以在终端执行下面的命令将 laravel/homestead box 添加到 Vagrant 中安装。你可能需要几分钟的时间来下载 box，因为它取决于你的网络连接速度：
#### 方式一
```shell
vagrant box add laravel/homestead
```
然后按3，选择virtualbox回车

#### 方式二
由于这个网络原因这个下载特别慢，提前把这个包放到百度放盘，可自行下载
链接: https://pan.baidu.com/s/1Cfk-KdFKY70ylGnP_Y-bkA  密码: s71j
这个下载后解压进入当前目录
```shell
vagrant box add metadata.json
```

### 安装 Homestead
你可以通过克隆代码来安装 Homestead。建议将代码克隆到你的「home」目录下的 Homestead 文件夹中，这样 Homestead box 就可以作为你的所有 Laravel 项目的主机：
```shell
git clone https://github.com/laravel/homestead.git ~/Homestead
```

因为 Homestead 的 master 分支并不是稳定的，你应该使用打过标签的稳定版本。您可以在 GitHub Release Page 上找到最新的稳定版。或者，你可以查看包含最新稳定版本的 release 分支：

```shell
cd ~/Homestead

git checkout release
```

一旦克隆 Homestead 代码完成以后，在 Homestead 目录中使用 bash init.sh 命令来创建 Homestead.yaml 配置文件。Homestead.yaml 文件将被放在 Homestead 目录中：
```shell
bash init.sh
```
### 配置共享文件夹#
Homestead.yaml 文件的 folders 属性里列出了所有与 Homestead 环境共享的文件夹。这些文件夹中的文件如果发生变更，它们会保持本地机器与 Homestead 环境之间同步。你可以根据需要配置多个共享文件夹：
```shell
folders:
    - map: ~/code/     // 本地代码根目录
      to: /home/vagrant/code // 虚拟机代码根目录
```

### 配置 Nginx 站点
对 Nginx 不熟悉？没关系。 sites 功能可以让你在 Homestead 上轻松的映射一个 "域名" 到一个文件夹。在 Homestead.yaml 文件中包含了一个简单的站点配置示例。同样，您可以根据需要为您的 Homestead 环境添加很多的站点。Homestead 可以为你正在开发的每个 Laravel 项目提供一个方便的虚拟化环境：

```shell
sites:
    - map: homestead.test
      to: /home/vagrant/code/project1/public
      php: "7.1"  // 指定php版本
```
如果你在 Homestead 虚拟机启动后更改了 sites 选项，你需要再次运行 vagrant reload --provision 命令去更新虚拟机上的 Nginx 配置

### 主机名解析#
```shell
192.168.10.10 homestead.test
```

### Vagrant常用命令
|命令行|说明|
|:----|:---|
|vagrant init|初始化 vagrant|
|vagrant up	|启动 vagrant|
|vagrant halt|关闭 vagrant|
|vagrant ssh|通过 SSH 登录 vagrant（需要先启动 vagrant）|
|vagrant provision|重新应用更改 vagrant 配置|
|vagrant destroy|删除 vagrant|   

