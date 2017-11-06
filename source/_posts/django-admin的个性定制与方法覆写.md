---
title: django admin的个性定制与方法覆写
date: 2017-02-13 20:18:51
tags:
- django
- modeladmin
- action
categories:
- python
- django
permalink: ModelAdmin-and-action-of-django-admin
---

## django modeladmin 个性定制与方法覆写
我喜欢django的原因有很多，最直接的一条是，她有一套强大的model系统，特别是有一套管理后台ModelAdmin，
让我在做项目时非常方便，但功能做多了，需求也就各式各样了，所以部分功能需要定制化。
这里就说一下我在项目过程中所遇到的，这些在官方文档中也都有体现。

<!--more-->
最常见的两项无非是modeladmin和actions的覆写和增加

[官网ModelAdmin](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/)

[官网Actions](https://docs.djangoproject.com/en/1.10/ref/contrib/admin/actions/)

### ModelAdmin

#### get_queryset
ModelAdmin上的get_queryset方法会返回管理网站可以编辑的所有模型实例的QuerySet。
简单来说就是返回一个从model中查询到的数据对象集合。
官网给出的示例，qs是获取到的数据集合，然后通过判断是否为管理员类型，对非管理员数据进行过滤：

```python
class MyModelAdmin(admin.ModelAdmin):
    def get_queryset(self, request):
        qs = super(MyModelAdmin, self).get_queryset(request)
        if request.user.is_superuser:
            return qs
        return qs.filter(author=request.user)
```
##### 想要在后台显示列表的序号而非数据库中的ID的实现
要显示ID比较简单，在list_display中加了id项即可，但这个id是数据在数据库中的id，如果有删除，那么就不是列表序列了。
我的思路是：通过get_queryset获取到整个列表，从而知道共有多少条数据

```python
class MyModelAdmin(admin.ModelAdmin):
    def get_queryset(self, request):
        qs = super(MyModelAdmin, self).get_queryset(request)
        self.qs = qs
        return qs
```

然后新建方法获取每次列表循环的obj，判断obj在列表中的位置
```python
def ids(self, obj):
        return len(self.qs) - list(self.qs).index(obj)
```
别忘了在list_display中增加方法名：
```python
list_display = ['ids', 'eventid', 'refpveventid', 'refclickeventid', 'updatetime']
```
果断妥妥的！
![index_id_show](http://oi1wvrjc2.bkt.clouddn.com/17-2-14/94925874-file_1487041725745_ba58.png)

##### 在列表中显示图片(参考网络)
先拼html代码，然后把显示路径和显示id做为参数传入，bingo! 写法真心灵活。
```python
def img_show(self, obj):
    # 扩展显示list_display
    s = u'<img src="{}face/{}.jpg"style="width:2em;height:2em">'
    return format_html(s, django.conf.settings.MEDIA_URL, str(face.id))
```
依然别忘了把方法加入list_display
```python
list_display = ['img_show']
```
更高端的用法，大神们自行研究吧～

### Actions

#### 增加通用action
##### 首先增加actions项
```python
actions = ['make_published']
```

##### 创建make_published方法
官方示例： 将获取到的queryset中的status字段统一置为p值。
```python
def make_published(modeladmin, request, queryset):
    queryset.update(status='p')
make_published.short_description = "Mark selected stories as published"
```
其中modeladmin为当前类的ModelAdmin，request为当前的Httprequest，queryset为包含用户所项对象的集合。

同时提供了`short_description`方法，用来自定义名称显示在actions选择的位置。

##### 想要复制选中的数据，并修改其中的部分字段
其实通过上面的方法，以经有眉目了；但还差一点，差哪呢，就是在post actions的时候，渲染一个template；
类似于delete_action点击后的页面效果。

这里可以借鉴下delete_selected的实现方式，源码位置：
`/home/hanz/autohome-venv2.7/local/lib/python2.7/site-packages/django/contrib/admin/actions.py`

这样就简单了，现在只需要增加一个template，可以把模板增加至admin的模板下，比如：`templates/admin/pvtest/db_copy_confirmation.html`

###### 修改数据处理部分
```python
def copy_action(self, request, queryset):
    if request.POST.get('post'):
        if perms_needed:
            raise PermissionDenied
        n = queryset.count()
        p_v = request.POST.get("pv-version")
        if n:
            for obj in queryset:
                obj_display = force_text(obj)
                obj.pk = None
                obj.pvconfig_id = p_v
                obj.save()
                self.log_addition(request, obj, obj_display)
            self.message_user(request, (u"成功复制了 %(count)s 个 %(items)s.") % {
                "count": n, "items": model_ngettext(self.opts, n)
            }, messages.SUCCESS)
```

复制数据其实还是挺简单的，只需要把pk置空，然后修改指定字段的值，重新保存，就OK了；

###### 修改渲染模板部分
默认actions的模板位置
`/home/hanz/python_workspace/flushcount/templates/admin/delete_confirmation.html`

修改actions调用的方法，确保调用的是你自己的复制方法；
`<input type="hidden" name="action" value="copy_action" />`

剩下的再做一点传参，提示语修改，基本也就OK了，如图：

![复制数据](http://oi1wvrjc2.bkt.clouddn.com/17-2-14/98045277-file_1487051259204_2634.png)

#### delete_action
覆写删除actions

数据的删除是比较危险的操作，如果你的系统是多用户的话，就应该避免多用户对数据所有数据都可以操作，所以，
你可以自己写delete_actions

##### 官网没有给出如何覆写delete_actions的方法，但看其他的actions，也应该差不到哪去。

###### 自己增加delete_action
相当于自己重写一个，使用delete_model方法
```python
def delete_model(self, request, obj):
    if request.user.is_superuser:
        obj.delete()
```

###### 修改显示文案
```python
delete_model.short_description = "delete selected"
```

###### 分权限删除
# todo

应该在判断数据是谁添加的，删除时只能是谁删除

目前没找到obj的user方法
