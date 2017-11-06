---
title: start-hexo
date: 2016-12-09 09:40:20
tags:
- hexo
---

# HEXO
## 环境配置
> 系统：ubuntu 16.04 64位

#### 安装git
```bash
sudo apt-get install git
```
#### 安装node
```bash
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -

sudo apt-get install -y nodejs
```
> 不同系统安装node参见：[这里](https://nodejs.org/en/download/package-manager/)

<!--more-->
#### 安装HEXO
```bash
npm install hexo-cli -g
npm install hexo --save
```
> hexo官网很详细，见[这里](https://hexo.io/zh-cn/)

至此，环境基本配置完毕。
----

## 初始化
```bash
hexo init myblog
```

## 教程参考：

> [官方教程](http://theme-next.iissnan.com/)
> [令狐葱](http://jiji262.github.io/)

> [Zhiho's Blog](http://zhiho.github.io/2015/09/29/hexo-next/)