---
title: 批量删除redis keys
date: 2016-12-28 18:49:19
tags:
- redis
categories:
- 数据库
- redis
permalink: batch-delete-redis-key
---
![redis](http://image.candymami.com/17-5-12/1777863-file_1494579468916_12846.png)

redis基本用法
====
## 连接redis库
```redis
redis-cli -h 127.0.0.1 -p 6070 -a password
```

## 选定数据库

默认数据库为0
```redis
select 0
```
<!--more-->
## 查询
### 单条查询
```redis
keys "key"
```

### 多条查询
使用通配符*
```redis
keys "key*"
```
## 删除
### 单独删除key
```shell
del keys
```

### 批量删除keys
利用linux中的xargs参数
```shell
redis-cli keys "mobile*" | xargs redis-cli del
//如果redis-cli没有设置成系统变量，需要指定redis-cli的完整路径
//如：/opt/redis/redis-cli keys "*" | xargs /opt/redis/redis-cli del
```

如果更改了redis端口，则需要指定端口-p
```shell
redis-cli -h 127.0.0.1 -p 6070 keys "mobile*" | xargs redis-cli -h 127.0.0.1 -p 6070 del
```

如果要指定redis库，则使用-n
```shell
//下面的命令指定数据序号为0，即默认数据库
redis-cli -h 127.0.0.1 -p 6070 keys "mobile*" -n 0 | xargs redis-cli -h 127.0.0.1 -p 6070 -n 0 del
```
### 全部删除
```shell
//删除当前数据库中的所有Key
flushdb

//删除所有数据库中的key
flushall
```

