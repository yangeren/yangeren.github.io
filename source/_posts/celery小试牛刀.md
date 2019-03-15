---
title: celery小试牛刀
date: 2017-05-02 15:51:34
tags: 
- celery
catetories:
- python
permalink: how-to-use-celery-running-task
thumbnail: http://image.candymami.com/17-5-2/87108123-file_1493712414064_6eb1.png
---

Celery小试牛刀
----

### 介绍

参考资料：

- [celery 官网](http://www.celeryproject.org/)
- [Python 并行分布式框架 Celery](https://blog.csdn.net/freeking101/article/details/74707619)


#### 异步执行

Celery是一个异步任务的调度工具。

Tasks can execute asynchronously (in the background) or synchronously (wait until ready).

任务可以异步（在后台）或同步执行（等到准备好）。

#### 组成部分

Celery的架构由三部分组成：
1. 消息中间件（message broker）
2. 任务执行单元（worker）
3. 任务执行结果存储（task result store）

![celery 组成部分](http://image.candymami.com/18-8-14/62830101.jpg)

#### 安装

根据以上的组成部分，需要安装以下几种：

初学推荐使用方案一，环境比较好搭

方案一：

- redis (message broker && task result store)
- celery (worker)

```bash
pip install redis
pip install celery
```

方案二：

- RabbitMQ (message broker)
- mysql (task result store)
- celery (worker)



### 应用

#### 单文件应用

```python
from celery import Celery

broker = 'redis://127.0.0.1:6378/5'
backend = 'redis://127.0.0.1:6379/6'

app = Celery('tasks', broker=broker, backend=backend)

@app.task
def add(x, y):
    return x + y
```

目录结构：
```shell
tree
.
├── __pycache__
│   └── tasks.cpython-35.pyc
├── tasks.py  # 单文件
└── tasks.pyc

```

#### 配置文件应用

[celery 配置项](http://docs.celeryproject.org/en/latest/userguide/configuration.html)

- 建个python包， proj

- 创建celery.py
```python
from celery import Celery

app = Celery('proj', include=['proj.tasks'])
app.config_from_object('proj.config')

if __name__ == '__main__':
    app.start()

```

- 创建config.py配置文件

```python
CELERY_RESULT_BACKEND = 'redis://127.0.0.1:6379/5'
BROKER_URL = 'redis://127.0.0.1:6379/6'
```

- 创建tasks.py 任务文件
```python
from proj.celery import app 

@app.task
def add(x, y):
    return x + y
```
#### 结合Django的Celery配置

### 启动服务

#### 启动worker

#### 启动beat


### 可执行方式

#### task

#### Scheduler

#### crontab
 
# 如何使用celery执行异步任务


# 如何使用celery beat执行定时任务
