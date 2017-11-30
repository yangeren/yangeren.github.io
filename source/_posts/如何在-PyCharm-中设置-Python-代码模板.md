---
title: 如何在 PyCharm 中设置 Python 代码模板
date: 2017-11-07 14:48:33
tags:
- pycharm
- setting
categories:
- python
thumbnail: http://oi1wvrjc2.bkt.clouddn.com/17-11-28/66017375.jpg
permalink: How-To-Set-Pycharm-Model
---


### 设置

> 设置 File > Settings > File and Code Template > Python Script


```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# Created by author on $DATE


if __name__ == '__main__':
    pass

```

可使用的环境变量： `${DATE}`, `${TIME}`等。