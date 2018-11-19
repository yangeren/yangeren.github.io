---
title: 使用Flask-Cache为页面增加缓存
date: 2018-11-19 14:05:02
tags:
- python
- flask-cache
categories:
- python
- flask
thumbnail:
permalink: Use-Flash-Cache-To-Quickly-Increase-The-Cache
---

使用Flash-Cache快速增加缓存
====

### 背景

#### 速度慢

#### 数据库压力大

#### 场景不需要展示实时数据

### 快速配置

#### 安装

`pip install Flask-Cache`

#### 初始化

官方给出两种方式：

第一种：
```python
from flask import Flask
from flask_cache import Cache

app = Flask(__name__)
cache = Cache(app,config={'CACHE_TYPE': 'simple'})
```

第二种：
```python
from flask import Flask
from flask_cache import Cache

cache = Cache(config={'CACHE_TYPE': 'simple'})
app = Flask(__name__)
cache.init_app(app)
```

#### Cached()

1. 只缓存path

```python
@app.route('/report', methods=['GET'])
@cache.cached(timeout=300)
def report():
    return "hello world"
```

2. 缓存path及参数（需要自建方法）

```python
from flask import jsonify
from urllib import parse

def cache_key():
    args = request.args
    key = request.path + '?' + parse.urlencode([
        (k, v) for k in sorted(args) for v in sorted(args.getlist(k))
    ])
    return key
    
@app.route('/report', methods=['GET'])
@cache.cached(timeout=300, key_prefix=cache_key)
def report():
    b = request.values.get("b")
    v = request.values.get("v")
    p = request.values.get("p")
    pc = request.values.get("pc")
    result = {"b":b, "v":v, "p":p, "pc":pc}
    return jsonify(result)

```

#### memoize()

自动按参数进行缓存

```python
@cache.memoize(timeout=50)
def big_foo(a, b):
    return a + b + random.randrange(0, 1000)
```

### 踩过的坑

#### 报引用错

1. 官方API中引用报错：

将：`from flask.ext.cache import Cache`

修改为：

`from flask_cache import Cache`

2. 执行过程中报错

`from flask.ext.cache import make_template_fragment_key`

此报错原因是新版本flask不支持引用flask.ext了，所以简单修改即可正常使用：

文件位置：

`/your-python-forder/site-packages/flask_cache/jinja2ext.py`

第33行修改为：

`from flask_cache import make_template_fragment_key`

### 测试

刷新接口后，可以看到增加缓存后响应时间减少还是很明显的。

> [官方API](http://www.pythondoc.com/flask-cache/)