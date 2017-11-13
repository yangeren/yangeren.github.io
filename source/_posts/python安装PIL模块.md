---
title: python安装PIL模块
date: 2016-12-29 10:59:36
thumbnail:
tags:
- PIL
categories:
- python
permalink: python-install-PIL-module 
---

python安装PIL模块
=====

> 因为要获取图片的信息，所以需要PIL模块来解析

[官方教程](http://effbot.org/imagingbook/)
<!--more-->
## 安装

### ubuntu

```shell
pip install -I --no-cache-dir -v Pillow
```

## 使用

### 引用
```python
from PIL import Image
```
### 简单应用

```python
from PIL import Image
# 打开
ima = Image.open("test.png")
# 获取尺寸\类型等
print ima.format, ima.size, ima.mode
#Out[19]: PNG (400, 300) RGB
```

### 加载网络图片
使用StringIO模块将文件写入内存，即伪装成file
```python
from PIL import Image
import StringIO
import requests
res = requests.get(url="http://car0.autoimg.cn/logo/brand/100/130549643705032710.jpg")
resBuff = StringIO.StringIO(res.content)
ima = Image.open(resBuff)
ima.size
#Out[19]: (100, 100)
```
