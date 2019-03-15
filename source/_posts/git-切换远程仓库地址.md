---
title: git 切换远程仓库地址
date: 2017-11-06 18:45:39
tags: 
- git
categories:
- linux
- git
permalink: git-switch-the-remote-repository-address
thumbnail: http://image.candymami.com/17-11-28/4082798.jpg
---

git 切换远程仓库地址
====

### 修改命令

git remote set-url origin url

### 先删后加

git remote rm origin
git remote add origin git@github.com:yours/mysite.git

### 修改config文件

如果你的项目有加入版本控制，那可以到项目根目录下，查看隐藏文件夹， 
发现.git文件夹，找到其中的config文件，就可以修改其中的git remote origin地址了。

