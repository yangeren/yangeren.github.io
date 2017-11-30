---
title: 在POST请求中，如何获取select的值
date: 2017-02-08 10:05:59
tags:
- django
categories:
- python
- django
---

## 从页面POST请求中，如何获取select中option的值

### GET方法中获取
通过GET方法传参，在request.GET中，可以获取到相对应的参数的值，
但POST方法中只能获取到input框中传的内容
如果真是这样的话，那就太恶心了，好在有stackoverflow

<!--more-->
### stackoverflow中的解决办法
[解决方案](http://stackoverflow.com/questions/39200802/django-post-get-options-from-select)

`request.POST.getlist("name")[0]`

在POST方法中居然还有个getlist，当时我debug时也没有看到有此方法，使用了一下，
果然可正常获取。

如图：

![request.POST.getlist](http://oi1wvrjc2.bkt.clouddn.com/django_requsts_post_getlist.png)

