---
title: 基于python环境的各类环境搭建
date: 2016-12-12 16:01:13
tags:
- python
- gunicorn
- django
- supervisor
- celery
categories:
- python
---
基于python环境的各类环境搭建
=====

## ---服务类---

|序号 |  功能|作用    |状态|
|:---|:-----|:---|:---|
|1|django跑起来|站点|完成|
|2|gunicorn跑起来|内部分发服务|完成|
|3|nginx跑起来|静态外部服务器|完成|
|4|supervisor跑起来|任务监控|完成|
|5|celery跑起来|定时任务|未完成|
|6|redis跑起来|nosql快速缓存|未完成|
|-|-|-|-|

|1|jenkins走起|testcase定时任务及结果|完成|

<!--more-->

## ---页面类---

|序号|功能|作用|状态|
|:---|----|----|---|
|1|前台页面做起来|把后台功能及数据展示做到前台页面上|部分完成|
|2|前台样式用起来|前台样式使用通用bootstrap样式包|未完成|
|3|---|---|---|

### 1. django跑起来

引用：

官方文档



#### 用到的功能：

模板分页：

https://mozillazg.com/2013/01/django-pagination-by-use-paginator.html

http://python.usyiyi.cn/django/topics/pagination.html





### 2. gunicorn跑起来

引用：

http://foofish.net/blog/18/django-deploy

http://michal.karzynski.pl/blog/2013/06/09/django-nginx-gunicorn-virtualenv-supervisor/



#### gunicorn超时时间设置

引用：

http://stackoverflow.com/questions/6816215/gunicorn-nginx-timeout-problem

脚本文件bin/gunicorn_start.sh

```python

NUM_WORKERS=3
TIMEOUT=120

exec gunicorn ${DJANGO_WSGI_MODULE}:application \
--name $NAME \
--workers $NUM_WORKERS \
--timeout $TIMEOUT \
--log-level=debug \
--bind=127.0.0.1:9000 \
--pid=$PIDFILE
```

### 3. nginx跑起来

引用：

http://blog.csdn.net/qq_24861509/article/details/45727983

http://www.111cn.net/sys/linux/79751.htm



### 4. supervisor跑起来

- 安装

`sudo apt-get install supervisor`

- 启动

```shell

service supervisor start/restart/stop

/etc/init.d/supervisor start/restart/stop

```

- 配置文件位置

`/etc/supervisor/supervisord.conf`

```

; supervisor config file



[unix_http_server]

#file=/var/run/supervisor.sock   ; (the path to the socket file)

file=/tmp/supervisor.sock	;

chmod=0700                       ; sockef file mode (default 0700)



[supervisord]

logfile=/var/log/supervisor/supervisord.log ; (main log file;default $CWD/supervisord.log)

pidfile=/var/run/supervisord.pid ; (supervisord pidfile;default supervisord.pid)

childlogdir=/var/log/supervisor            ; ('AUTO' child log dir, default $TEMP)



; the below section must remain in the config file for RPC

; (supervisorctl/web interface) to work, additional interfaces may be

; added by defining them in separate rpcinterface: sections

[rpcinterface:supervisor]

supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface



[supervisorctl]

#serverurl=unix:///var/run/supervisor.sock ; use a unix:// URL  for a unix socket

serverurl=unix:///tmp/supervisor.sock ;



; The [include] section can just contain the "files" setting.  This

; setting can list multiple files (separated by whitespace or

; newlines).  It can also contain wildcards.  The filenames are

; interpreted as relative to this file.  Included files *cannot*

; include files themselves.



[include]

files = /etc/supervisor/conf.d/*.conf

```



- 增加任务

`/etc/supervisor/conf.d/autohome_celery.conf`

```config

[program:autohome_celery]

directory = /home/hanz/workspace/PycharmProjects/autohome_data_site

command = /home/hanz/workspace/my-venv2.7/bin/python manage.py celery worker -c 6 -l debug

autostart=true

autorestart=true

startsecs=3

user = hanz                                                          ;

stdout_logfile = /home/hanz/workspace/PycharmProjects/autohome_data_site/logs/autohome_celery.log

redirect_stderr = true

```

- 添加web显示页面

```config

[inet_http_server]         ; inet (TCP) server disabled by default

port=9001        ; (ip_address:port specifier, *:port for all iface)

username=admin             ; (default is no username (open server))

password=123               ; (default is no password (open server))

```

引用：

http://www.cnblogs.com/gsblog/p/3730293.html

http://www.cnblogs.com/yjf512/archive/2012/03/05/2380496.html

http://www.cnblogs.com/maseng/p/4670473.html

http://liyangliang.me/posts/2015/06/using-supervisor/



### 5. celery跑起来

- 独立celery+python脚本



- django-celery替代celery，在django项目中的使用方法



- 监控界面：flower

1. 安装：

`pip install flower`

2. 使用方法：

运行服务打开http://localhost:5555：

`flower --port=5555`

或者从celery运行：

`celery flower --address=127.0.0.1 --port=5555`

Broker URL 和其他配置选项能够通过一个标准的celery选项：

`celery flower --broker=amqp://guest:guest@localhost:5672//`

在setting中添加：

```bash
djcelery.setup_loader()
CELERY_TIMEZONE = 'Asia/Shanghai'
BROKER_URL = 'redis://127.0.0.1:6379/8'
CELERY_RESULT_BACKEND = 'redis://127.0.0.1:6379/9'
CELERYBEAT_SCHEDULER = 'djcelery.schedulers.DatabaseScheduler'
CELERY_IMPORTS = ("apps.app1.tasks", "autohome.views")
```

想要使用任务 ，一定要imports对应的py文件，修改后需要重启celery服务

引用：

http://flower-docs-cn.readthedocs.io/zh/latest/install.html

-

引用：
> http://www.celeryproject.org/
> http://www.liaoxuefeng.com/article/00137760323922531a8582c08814fb09e9930cede45e3cc000
> http://www.jianshu.com/p/1840035cb510
> http://www.dongwm.com/archives/how-to-use-celery/
> http://liuzxc.github.io/blog/celery/
http://opslinux.com/2015/09/22/celery%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1%E5%AE%9E%E8%B7%B5/
http://blog.csdn.net/vintage_1/article/details/47664297


django celery

> http://blog.csdn.net/vintage_1/article/details/47664297
> http://maslino.website/post/celery-documentation-django.html



### 6. jenkins走起

- jenkins 邮件配置

引用：

> http://www.cnblogs.com/amosli/p/3549918.html
> http://www.cnblogs.com/GGHHLL/p/jenkins.html

