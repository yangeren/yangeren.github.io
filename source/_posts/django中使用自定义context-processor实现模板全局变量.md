---
title: django中使用自定义context processor实现模板全局变量
date: 2016-12-30 11:34:04
tags:
- django
- template
categories:
- python
- django
permalink: django-use-custom-context-processor-achieve-template-global-variables
---

django中使用自定义context processor实现模板全局变量
====

## 在视图中定义全局变量方法
<!--more-->
### 在每个页面显示google指向的ip
```python
import socket
def context_proc(request):
    """获取google的ip指向，每个页面都需要显示"""
    env = socket.getaddrinfo("www.google.com", "http")[0][4][0]
    return {
    "env": env,
    }
```

### 如果有多个全局变量要显示，在return中添加即可
```python
return {
"evn": evn,
"host": host,
"user": request.user,
}
```

## 将全局变量方法添加至setting中的Templates

### 添加位置
```python
TEMPLATES = [
    {
        'BACKEND': "...",
        "...": "...",
        "OPTIONS": {
            "context_processors": [
                "...",
                "views.context_proc",
            ]
        }
    }
]
```

## 在模板中引用即可

### head.html
```html
<a href="#">{% block env %}{% endblock %}</a>
```

### index.html
```html
{% block env %}
{{ env }}
{% endblock %}}
```

## 关于setting.py

setting中的各种设置参数都是对global_settings的覆写
可以引用后进行覆写
### 引用
```python
from django.conf import global_settings
```

### 位置
一般位于
```python
python根目录/site-package/django/conf/global_setting.py
```
### 覆写
```python
ALLOWED_HOSTS = []
```