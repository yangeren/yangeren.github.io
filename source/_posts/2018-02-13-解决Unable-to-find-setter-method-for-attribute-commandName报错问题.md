---
title: '解决Unable to find setter method for attribute: commandName报错问题'
date: 2018-02-13 14:07:54
tags:
- spring
categories:
- java
- spring
thumbnail:
permalink:
---

Unable to find setter method for attribute: commandName
=====

### 解决过程

#### 发生原因

根据Spring + MVC学习指南（第2版）第5章的例子进行跑项目，发现始终报一个错误：
`Unable to find setter method for attribute: commandName`
根据报错提示，为BookAddForm.jsp中的form标签出错了，从网上找各种方法不得解


#### 解决方法

想本着解决的态度看一下标签文件是怎么写的，于是点进`spring-form.tld`文件，
搜索了下`commandName`，于是看到：
```tld
		<attribute>
			<description>DEPRECATED: Use "modelAttribute" instead.</description>
			<name>commandName</name>
			<required>false</required>
			<rtexprvalue>true</rtexprvalue>
		</attribute>
```

原来`commandName`过旧，已经被`modelAttribute`替代了，于是更改jsp文件中的标签，解决。

