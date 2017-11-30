---
title: 如何将一个已经存在的目录转换为GIT项目并托管到github或coding仓库
date: 2017-05-02 17:07:35
tags: 
- git
categories:
- linux
- git
permalink: how-to-add-exist-project-to-github-or-coding
thumbnail: http://oi1wvrjc2.bkt.clouddn.com/17-5-2/93220999-file_1493717085156_d3c4.png
---

# 如何将一个已存在的目录转换为一个 GIT 项目并托管到 GITHUB/CODING 仓库

## 背景：
将一个本地维护的项目，转换成git项目，并托管在github上。

## 步骤：

### 初始化本地项目目录
```bash
git init
```
此时会在目录中创建一个`.git`的隐藏目录

### 将所有的文件放入新的仓库中
```bash
git add .
```
如果你的本地已经有`.gitignore`目录，则按照此文件的规则进行排除文件；
单独添加文件，则把`.`替换为文件名。

### 将添加的文件提交到仓库
```bash
git commit -m "commit init"
```

### 获取远程仓库地址
登陆[github](https://github.com)或者[coding.net](https://coding.net)，获取远程仓库；

此时尽量先不要初始化README和LICENSE文件，防止文件冲突

获取`https://github.com/your-proj-name.git`类似的地址；

### 回到本地目录，将本地创建关联到远程仓库
```bash
git remote add origin https://github.com/your-proj-name.git
```

运行以下命令查看结果：
```bash
git remote -v
```

### 提交代码到远程创建 push
```bash
git push origin master
```

此时进入远程页面查看代码是否成功push即可。
