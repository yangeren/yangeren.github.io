---
title: linux下一句话Kill进程
date: 2017-06-08 17:31:15
tags: 
- pgrep
- kill
categories:
- linux
permalink: many-way-to-killed-ps-in-linux
---
### 查询tomcat进程

![Control-Linux-Processes](http://oi1wvrjc2.bkt.clouddn.com/17-9-26/39733243.jpg)
```bash
ps -ef | grep tomcat
ps -aux | grep tomcat
```

### 只查看进程pid
```bash
pgrep -f tomcat
```


### tomcat server killed

```bash
kill -s 9 `pgrep -f tomcat`
```

> 引用自[这里](http://blog.csdn.net/smarxx/article/details/6664219)