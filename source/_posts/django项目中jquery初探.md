---
title: django项目中jquery初探
date: 2017-01-05 01:46:24
tags:
- django
- jquery
categories:
- python
- django
permalink: jquery-in-django-project
---
django项目中jquery初探
====
> 之前一直关注后端的技术与功能，今天在考虑自动化平台执行时，突然发现，前后端交互这块是个强需求，我想要的效果是，前端只需要点击，即可出结果，不用刷新页面。这样就不可避免的用到了ajax技术，那最简单的就是使用jquery框架了，这里简单的应用，记录一下。
<!--more-->

目前要做一个表格，点击按钮请求表格中的接口，并把相关数据通过ajax方式传输，
从而达到不用刷新页面显示请求结果的目地。

### 视图view.py
> 获取模板传过来的url做为参数，对url进行处理后，包装数据回传给模板，显示。

```python
import requests
from django.http import HttpResponse, JsonResponse
import json

def req_func(request):
    """
    请求方法
    :param request:
    :return:
    """
    if request.method == "GET":
        url = request.GET["url"].strip()
        try:
            res = requests.get(url=url, timeout=10)
            code = res.status_code
            time = res.elapsed.total_seconds()
            if "application/json" in res.headers.get('Content-Type'):
                content = res.json()
            else:
                content = "not json type"
            result = {"code": code, "time": time, "content": json.dumps(content, indent=4)}
            return JsonResponse(result)
        except Exception as e:
            return HttpResponse(e)
    else:
        pass
```

### template
> 模板中使用jquery的get方法，将数据组装后传，调用后端方法，进行请求，并接收回传结果并显示。
我是通过对id后面的数值判断拿的参数值，应该还有其他的方法，后期再研究一下。

```django
{% extends "base/head.html" %}
{% block title %}
    {{ title }} detail
{% endblock %}
{% block env %}
    {{ env }}
{% endblock %}
{% block script %}
    <script type="application/javascript">
        $(document).ready(function () {
            $("button").click(function () {
                {#                alert($(this).attr("id").slice(3));#}
                var nowid = $(this).attr("id").slice(3);
                $.get("{% url "interface:reqfunc" %}",
                        {
                            url: $("#comurl" + nowid).text(),
                        },
                        function (ret) {
                            $("#code" + nowid).text(ret.code);
                            $("#time" + nowid).text(ret.time + " S");
                            $("#content" + nowid).text(ret.content);
                        });
            });
            $("#hidden").click(function () {
                $("#cont").toggle(speed, callback);
            })
        });
    </script>
{% endblock %}
{% block content %}
    <h1>{{ url.name }}</h1>
    <table class="table table-bordered">
        <thead>
        <tr>
            <th>num</th>
            <th>接口</th>
            <th>code</th>
            <th>time</th>
            <th>run</th>
        </tr>
        </thead>
        <tbody>
        {% for i in params %}
            <tr>
                <td>{{ forloop.counter }}</td>
                <td><span
                        id="comurl{{ forloop.counter }}">{{ url.schame }}{{ url.domain }}{{ url.version.version }}{{ url.path }}?{{ i.query }}</span>
                </td>
                <td><span id="code{{ forloop.counter }}" class="label label-danger"></span></td>
                <td><span id="time{{ forloop.counter }}" class="label label-danger"></span></td>
                <td>
                    <button id="run{{ forloop.counter }}">run</button>
                </td>
                                <table id="cont" border="1">
                                    <tr>
                                        {% autoescape off %}
                                        <td id="content{{ forloop.counter }}"></td>
                                        {% endautoescape %}
                                    <td id="hidden">hidden</td>
                                    </tr>
                                </table>
            </tr>
        </tbody>
    </table>
{% endblock %}
```

### urls.py
> 需要将视图层的数据做url指向

```django
url(r'^reqfunc/$', views.req_func, name="reqfunc"),
```

目前就这些，参考资料后补。