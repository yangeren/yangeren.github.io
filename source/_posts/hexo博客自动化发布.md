---
title: hexo博客自动化发布
date: 2017-11-06 10:43:51
tags: 
- auto deploy
categories:
- hexo
permalink: hexo-blog-using-travis-CI-automation-release
---

Hexo借助travis-ci自动发布博客
====

![travis ci](http://oi1wvrjc2.bkt.clouddn.com/17-11-6/87226994.jpg)

###认证

#### 安装travis
```bash
gem install travis
```

#### 生成source密钥
```bash
travis encrypt -r owner/repo GH_Token=Your_Personal_Access_Token
```


### 遇到问题

#### 安装hexo-cli后无法使用

#### 无法push到仓库

