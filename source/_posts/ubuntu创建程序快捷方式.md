---
title: ubuntu创建程序快捷方式
date: 2017-05-08 15:26:03
tags: 
- ubuntu
categories: 
- linux
- ubuntu
permalink: how-to-create-a-program-shotcut-in-ubuntu

---

## 快捷方式的位置
Ubuntu快捷方式的位置有两个
1. `/usr/share/applications`
2. `～/.local/share/applications`

## 后缀名
所有的快捷方式的后缀名都以`.desktop`结束

## 快捷方式内容
以pycharm为例，内容如下：

```bash
[Desktop Entry]
Version=1.0 //版本号
Type=Application //  
Name=PyCharm2017 // 显示名称
Icon=/home/yourname/programs/pycharm-2017.1.1/bin/pycharm2017.png // 图标图片路径
Exec="/home/yourname/programs/pycharm-2017.1.1/bin/pycharm.sh" %f // 程序执行文件
Comment=Develop with pleasure!
Categories=Development;IDE;
Terminal=false //是否终端执行
StartupWMClass=jetbrains-pycharm
```

## 快捷方式的权限
权限如下：
```bash
-rw-------
```

## 运行
将文件放入指定位置后，点击ubuntu左上角搜索输入名称，即可显示；
如图：
![](http://image.candymami.com/17-5-8/19586603-file_1494231696632_14626.png)