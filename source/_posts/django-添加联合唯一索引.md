---
title: django 添加联合唯一索引
date: 2017-02-16 16:25:29
tags: 联合唯一索引
categories: 
- django
permalink: Django-add-contact-to-unique-index
thumbnail:
---

## Django 添加联系唯一索引方法
> 联合唯一索引，指的是，多个键值相同时保持唯一，添加方法如下：


```python
from django.db import models
class MyModel(models.Model):
    name = models.CharField(max_length=200, verbose_name="name")
    version = models.IntegerField(max_length=100, verbose_name="version")

    class Meta():
        verbose_name = "mymodel"
        verbose_name_plural = "mymodel"
        unique_together = (("name", "version"),)
```