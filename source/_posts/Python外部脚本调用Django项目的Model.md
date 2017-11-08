---
title: Python外部脚本调用Django项目的Model
date: 2017-11-02 16:35:59
thumbnail: http://oi1wvrjc2.bkt.clouddn.com/17-11-8/49753235.jpg
tags: 
- python
- django
permalink: Python-external-script-calls-the-Django-project-model-table
categories: 
- django
---

Python外部脚本调用Django项目的Model
====

![django cycle](http://oi1wvrjc2.bkt.clouddn.com/17-11-2/41607477.jpg)
## 在django中，python脚本状态下进行调用models

我们会有这样的场景，开着项目代码，想执行一个功能简单的脚本，但又想使用django简单的models调用，
两种方法，均可使用：

### python console

> 十分简单的脚本，推荐这种调试方式

可以选择在`python console`下进行调试脚本，

```bash
python manage.py shell
```

因为在此环境下，自动加载了`django`的项目路径至环境变量中
如果你使用的`pycharm`，可以看一下他的python console:

```python
import sys; print('Python %s on %s' % (sys.version, sys.platform))
import django; print('Django %s' % django.get_version())
sys.path.extend(['/home/yours/workspace/PycharmProjects/your_blog', '/home/hanz/programs/pycharm-2017.1.1/helpers/pycharm', '/home/yours/programs/pycharm-2017.1.1/helpers/pydev'])
if 'setup' in dir(django): django.setup()
import django_manage_shell; django_manage_shell.run("/home/yours/workspace/PycharmProjects/your_blog")
Python 2.7.10 (default, Oct 14 2015, 16:09:02) 
[GCC 5.2.1 20151010] on linux2
Django 1.9.5

```

### 在python脚本中引入环境变量

> 如何脚本有一定工作量，建议使用此种方式

```python
# coding=utf-8
import os
import sys
import django
sys.path.append('/home/yous/workspace/PycharmProjects/your_blog') # 将项目路径添加到系统搜寻路径当中
os.environ['DJANGO_SETTINGS_MODULE'] = 'your_blog.settings' # 设置项目的配置文件
django.setup() # 加载项目配置
```

加入以上代码后，脚本可正常引用项目中的model

```python
from your_proj.models import Series
series_ids = Series.objects.values("series_id")
```

完美！

