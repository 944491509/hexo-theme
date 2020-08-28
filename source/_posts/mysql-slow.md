---
title: 开启mysql慢查询
date: 2020-12-05 10:36:35
tags: [mysql] 
---
## 简介
MySQL的慢查询，全名是慢查询日志，是MySQL提供的一种日志记录，用来记录在MySQL中响应时间超过阀值的语句.


## 参数说明
- slow_query_log 慢查询开启状态
- slow_query_log_file 慢查询日志存放的位置（这个目录需要MySQL的运行帐号的可写权限，一般设置为MySQL的数据存放目录）
- long_query_time 查询超过多少秒才记录

## 设置步骤

### 1. 查看慢查询的相关参数

```shell
mysql> show variables like 'slow_query%';
+---------------------+-----------------------------------+
| Variable_name       | Value                             |
+---------------------+-----------------------------------+
| slow_query_log      | OFF                               |
| slow_query_log_file | /var/lib/mysql/homestead-slow.log |
+---------------------+-----------------------------------+

mysql> show variables like 'long_query_time';
+-----------------+-----------+
| Variable_name   | Value     |
+-----------------+-----------+
| long_query_time | 10.000000 |
+-----------------+-----------+
```
 
 ### 2.设置方法
#### 方法一:全局变量设置

将 slow_query_log 全局变量设置为“ON”状态
```shell
mysql> set global slow_query_log='ON';
```

设置慢查询日志存放的位置
```shell
mysql> set global slow_query_log_file='/usr/local/mysql/data/slow.log';
```

查询超过1秒就记录
```shell 
mysql> set global long_query_time=1;
```

#### 方法二
修改配置文件my.cnf，在[mysqld]下的下方加入
```
slow_query_log = ON
slow_query_log_file = /usr/local/mysql/data/slow.log
long_query_time = 1
```
### 3.重新加载MySQL配置
```shell
sudo service mysql reload
```
### 4.查看设置后的参数

修改完配置后要新开窗口去查看

```shell 
mysql> show variables like 'slow_query%';
+---------------------+--------------------------------+
| Variable_name       | Value                          |
+---------------------+--------------------------------+
| slow_query_log      | ON                             |
| slow_query_log_file | /usr/local/mysql/data/slow.log |
+---------------------+--------------------------------+

mysql> show variables like 'long_query_time';
+-----------------+----------+
| Variable_name   | Value    |
+-----------------+----------+
| long_query_time | 1.000000 |
+-----------------+----------+
```

## 测试
### 1.执行一条慢查询SQL语句
```shell
mysql> select sleep(2);
```
### 2.查看是否生成慢查询日志
```code
ls /usr/local/mysql/data/slow.log
```
如果日志存在，MySQL开启慢查询设置成功！

