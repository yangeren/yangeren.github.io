---
title: 使用urlparse修改url
date: 2017-11-06 09:39:20
tags: 
- python
- urlparse
categories:
- python
permalink: Use-urlparse-to-change-the-url
---

使用urlparse修改url中的各各部分
=====

## 举例来说明

![](http://oi1wvrjc2.bkt.clouddn.com/17-9-26/66144228.jpg)

`url="https://stackoverflow.com/questions/21628852/changing-hostname-in-a-url"`

```python
# 引入urlparse包
In [2]: import urlparse 

# 实例化url链接，变为urlparse对象
In [3]: u = urlparse.urlparse(url)

# 查看u中所包含的
In [4]: u
Out[4]: ParseResult(scheme='https', netloc='stackoverflow.com', path='/questions/21628852/changing-hostname-in-a-url', params='', query='', fragment='')

# 将scheme进行替换，并赋值给u_change_scheme
In [5]: u_change_scheme = u._replace(scheme="ftp")

# 查看u_change_scheme对象
In [6]: u_change_scheme
Out[6]: ParseResult(scheme='ftp', netloc='stackoverflow.com', path='/questions/21628852/changing-hostname-in-a-url', params='', query='', fragment='')

# geturl查看，已经变为想更改的scheme
In [7]: u_change_scheme.geturl()
Out[7]: 'ftp://stackoverflow.com/questions/21628852/changing-hostname-in-a-url'

# 可一次更改多个值
In [8]: u_change_scheme_and_netloc = u._replace(scheme="http", netloc="candypapi.com")

# 查看geturl
In [9]: u_change_scheme_and_netloc.geturl()
Out[9]: 'http://candypapi.com/questions/21628852/changing-hostname-in-a-url'

```

> 参考：[changing-hostname-in-a-url](https://stackoverflow.com/questions/21628852/changing-hostname-in-a-url)
