---
title: linux免密码认证配置
date: 2016-12-12 11:26:53
tags: 
- linux
- ubuntu
- rsa认证
categories:
- linux
permalink: Server-password-free-certification
---
# 服务器免密码认证
----

> 配置过程很简单，但每次都忘记一些细节，故做个笔记

<!--more-->
### 单向免密码登陆

服务器A，PC机B。

#### 生成
一般情况下，linux系统都会在你的目录下`~/`有`.ssh`文件夹，如果没有，则需要生成：
```shell
ssh-keygen
```
如果不需要特殊配置，直接一路yes就可以了
这时候再去看，就已经有了：
```shell
ls ~/.ssh

id_rsa  id_rsa.pub  known_hosts
```

#### 认证
##### 在目标服务器新建认证文件
在需要免密码的服务器上，即A服务器中的`~/.ssh/`下新建文件：
```shell
cd ~/.ssh
touch authorized_keys
```
##### 获取公钥
将操作机pc中的公钥`id_rsa.pub`取出来：
```shell
cat ~/.ssh/id_rsa.pub
```

##### 完成认证
将打印出来的`id_rsa.pub`中的内容粘贴至服务器A的authorized_keys中，保存即可。

##### 脚本
如果需要认证多台机器，可以自行将以上过程做成小脚本，我懒，没做。。。

[linux下小工具参考](http://kuanghy.github.io/2016/09/01/linux-softwares)

