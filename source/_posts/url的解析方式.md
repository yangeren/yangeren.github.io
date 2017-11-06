---
title: url的解析方式
date: 2017-02-20 09:23:58
tags:
- python
- url
categories:
- python
permalink: Analysis-of-the-url
---

## URL的解析方式，简单有效
![](http://oi1wvrjc2.bkt.clouddn.com/17-9-26/66144228.jpg)
### URL的组成
#### 举例url：
<!--more-->
`https://www.domain.org/this-is-path?username=w&password=p`
- `scheme` 如：https://
- `domain` 如：www.domain.org
- `path` 如：this-is-path
- `params` 如：username=w&password=p `params` 数据成对出现

### 解析URL
#### 引用包urlparse
```python
import urlparse
url = "https://www.domain.org/this-is-path?username=w&password=p"
pa = urlparse.urlparse(url)
print pa
```
将url解析成了对象，如下：
`Out[28]: ParseResult(scheme='https', netloc='www.domain.org', path='/this-is-path', params='', query='username=w&password=p', fragment='')`

#### 获取数据
```ipython
In [29]: pa.scheme
Out[29]: 'https'

In [30]: pa.netloc
Out[30]: 'www.domain.org'

In [34]: pa.hostname
Out[34]: 'www.domain.org'

In [37]: pa.path
Out[37]: '/this-is-path'

In [38]: pa.query
Out[38]: 'username=w&password=p'
```

#### 如何将参数格式化成dict类型？
```ipython
In [40]: urlparse.parse_qs(pa.query)
Out[40]: {'password': ['p'], 'username': ['w']}

In [41]: dict(urlparse.parse_qsl(pa.query))
Out[41]: {'password': 'p', 'username': 'w'}

```

#### 带参数据GET请求
```python
import requests
url = "https://www.domain.org/this-is-path"
params = {
"username": "w",
"password":"p",
}
res = requests.get(url=url, params=params)
```

#### 如果你有一段raw形式的参数要进行请求，可以使用parse_qs进行格式化后请求
```python
import requests
import urlparse
url = "https://www.domain.org/this-is-path"
p = "username=w&password=p"
params = urlparse.parse_qs(p)
res = requests.get(url=url, params=params)
```