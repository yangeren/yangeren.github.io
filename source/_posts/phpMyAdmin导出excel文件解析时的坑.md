---
title: phpMyAdmin导出excel文件解析时的坑
date: 2017-02-08 14:17:38
tags: 
- python
categories: 
- python
permalink: phpMyAdmin-export-excel-file-analysis
---

## phpMyAdmin导出excel文件，在解析时遇到的问题

### phpMyAdmin
很多人都喜欢使用的WEB数据库管理系统，前几年非常流行，刚好我要查询对方数据库，
他们就用的这个

### 需求
我的要求很简单，从他们库中导出数据文件，excel即可，然后自己解析，再进行处理数据
<!--more-->

### 坑
解析excel，当然首选简单好用的包，xlrd就很不错;
#### xlrd
但在读取时，总会报错；错误如下：
##### 读取
```python
import xlrd
workbook = xlrd.open_workbook("/home/hanz/book1.xls")
```

结果是，报错如下：
```errorcode
xlrd.biffh.XLRDError: Unsupported format, or corrupt file: Expected BOF record;
found '<html r'"
```

#### 其他解析excel包
之后又试了pandas，巨大无比的一个解析数据的包，但同样无法解析。。。

### 分析
#### 根据报错分析原因
根据xlrd报的错，猜了一下原因，有可能导出的excel文件并不是正规的excel保存的那种；
里面的<html就能看出来，中间是包含有html代码的，于是，用文本打开了看了一下，果然：

![phpmyadmin-export-excel-file](http://image.candymami.com/17-2-8/44364820-file_1486535637317_11f27.png)

把后缀改为.html，直接就能浏览器打开了。。。果然是伪excel文件。。。

### 解决方案
知道原因了，解决起来就简单了
#### 思路
如果是html文件，那首选html解析的包就可以，不用去做正则匹配，这类解析包也有很多，比如：

[beautifulsoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/)

[pyquery](https://pythonhosted.org/pyquery)

#### 选用
在这里选用我的宗旨就是方便，人生苦短嘛。
所以就用pyquery喽。
语法非常方便！～
pyquery完全API点击这里：[PyQuery-API](https://pythonhosted.org/pyquery/api.html)
```python
from pyquery import PyQuery
f = PyQuery("/home/hanz/o12_adsf.xls")
doo = f("tr")
doo.text()
```