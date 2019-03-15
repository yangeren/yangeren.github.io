---
title: ubuntu下使用Thunderbird连接Exchange邮件服务器
date: 2017-01-03 10:26:26
tags:
- ubuntu
- Thunderbird
categories:
- linux
---

ubuntu下使用Thunderbird连接Exchange邮件服务器
====

## 背景
公司为了加强安全性，故邮件服务器改为了exchange，这样一来，需要修改下自己邮件客户端及配置；
exchange的设置跟普通的126,163又有些区别，这里记录一下，也方便ubuntu的用户。

## 使用插件
<!--more-->
插件下载地址：[exquilla](https://addons.mozilla.org/zh-CN/thunderbird/addon/exquilla-exchange-web-services/)

插件安装和firefox浏览器一样，非常方便
免费使用60天

## 破解
60天肯定是不够用的，从网上找了个破解脚本，给力！

脚本下载：[下载](http://image.candymami.com/generater_license.py)

### 破解方法，引用文章
> 可以看到，注册码被用逗号分成了四个部分：
1. 第一部分是注册类型，EX0是免费给的试用类型，我不知道EX1、EX2是什么情况，但试了下，EX1是可以用的
2. 第二部分是邮件，*@*是免费给的60天试用的，这里要填有效的Exchange邮箱，可以在选项里Valid Emails里看到
3. 第三部分是license过期日期。
4. 第四部分是校验码，分别是前三个部分再加上356B4B5C算出来的MD5值。

>例如，注册类型EX1、Exchange邮箱i@ssfighter.com，到期日期2015-01-18，可以计算出MD5值为：

>MD5(EX1,i@ssfighter.com,2015-01-18,356B4B5C)=
5253dbb7d2b5a6e152974b2003025ba9

>用计算出的MD5值作为注册码的最后一部分即可注册成功。

参考文章：[破解方法](http://blog.ssfighter.com/2015/01/exquilla-31-0-crack/)

## 设置
### 面包屑
> 工具 >> ExQuilla for Microsoft Exchange >> ExQuilla license options

### 如图
![设置](http://image.candymami.com/2017-01-03%2011-19-20%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE1.png)


