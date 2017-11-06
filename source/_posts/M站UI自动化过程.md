---
title: M站UI自动化过程
date: 2016-12-13 11:37:13
tags:
- python
- selenium
- Remote
- UI自动化

categories:
- test

---

M站UI自动化过程
====
![](http://oi1wvrjc2.bkt.clouddn.com/17-5-9/20021003-file_1494301348754_cb1.png)
### 环境配置

#### 安装python

```
官网下载，linux与apple免安装
```

#### 创建沙盒环境

```shell
virtualenv myvenv
```
<!--more-->
#### 切换沙盒环境

```shell
source myvenv/bin/activate
```

#### 安装selenium包

```bash
pip install selenium
```

#### 下载chromedriver，用来支持webdriver调用chrome

```url
http://chromedriver.storage.googleapis.com/2.22/chromedriver_linux64.zip
```

#### 将chromedriver路径加入环境变量
> windows和linux方式有所不同，请自行google。

*若不使用chrome的emulate devices，则不用执行关于selenium-server的步骤*

#### 下载selenium-server服务，做remote服务，可启动chrome的mobile形式
地址如下
[selenium-server](http://selenium-release.storage.googleapis.com/2.53/selenium-server-standalone-2.53.1.jar)


### remote服务启动

#### 启动selenium-server服务

两种形式：

##### 通过脚本启动

```shell
gnome-terminal -t "title-name"gnome-terminal -x bash -c "/usr/bin/java -jar /home/hanz/download/selenium-server-standalone-2.53.1.jar; exec bash;"
```

##### 直接终端启动

```shell
/usr/bin/java -jar /home/hanz/download/selenium-server-standalone-2.53.1.jar
```

#### 脚本中启动chrome_mobile_emulation

```python
mobile_emulation = {"deviceName": "Google Nexus 5"}
chrome_options = webdriver.ChromeOptions()
chrome_options.add_experimental_option("mobileEmulation", mobile_emulation)
self.driver = webdriver.Remote(command_executor='http://127.0.0.1:4444/wd/hub',
                               desired_capabilities=chrome_options.to_capabilities())
```

### selenium 基本语法

#### 网上有成熟的各路教程，请参考，在最后我附了一些简明的教程链接

#### webdriver常用方法

```python

 'add_cookie', 在当前的会话添加一个cookie

 'application_cache', 返回一个对象ApplicationCache与浏览器的应用程序缓存交互

 'back', 返回至上一个浏览历史

 'capabilities', remote参数

 'close', 关闭

 'command_executor', remote webdriver参数

 'create_options', chrome webdriver参数

 'create_web_element', 创建具有指定element_id网络元素。

 'current_url', 当前页面的url

 'current_window_handle', 返回当前窗口的句柄

 'delete_all_cookies', 删除所有Cookie会话的范围

 'delete_cookie',删除指定名称的单一的cookie。

 'desired_capabilities', 返回正在使用当前所需功能的驱动程序

 'error_handler', 用于处理错误errorhandler.ErrorHandler对象。

 'execute',  发送到由一个command.CommandExecutor要执行的命令。

 'execute_async_script',  异步执行当前窗口/帧的JavaScript。

 'execute_script', 同步地执行在当前窗口/帧的JavaScript。

 'file_detector',

 'file_detector_context',

 'find_element',

 'find_element_by_class_name',通过查找类名的元素。

 'find_element_by_css_selector',通过查找CSS选择器的元素。

 'find_element_by_id',通过ID查找元素。

 'find_element_by_link_text', 通过查找链接文本的元素。

 'find_element_by_name',通过名称查找元素。

 'find_element_by_partial_link_text',其链接文本的部分匹配查找元素。

 'find_element_by_tag_name',查找按标签名称的元素。

 'find_element_by_xpath',与XPath查找元素。

 'find_elements',

 'find_elements_by_class_name',

 'find_elements_by_css_selector',

 'find_elements_by_id',

 'find_elements_by_link_text',

 'find_elements_by_name',

 'find_elements_by_partial_link_text',

 'find_elements_by_tag_name',

 'find_elements_by_xpath',

 'forward',前进了一步在浏览器历史记录。

 'get', 加载在当前浏览器会话的网页。

 'get_cookie',按名称获取一个Cookie。如果找到，无如果不返回cookie。

 'get_cookies',返回一组词典，对应的cookie在当前会话中可见。

 'get_log',获取日志对于给定的日志类型

 'get_screenshot_as_base64',获取当前窗口的截图为Base64编码的字符串 这是在以HTML嵌入的图像是有用的。

 'get_screenshot_as_file',获取当前窗口的截图。如果返回FALSE 任何IO错误，否则返回True。使用完整路径在你的文件名。

 'get_screenshot_as_png',获取当前窗口的屏幕快照作为二进制数据。

 'get_window_position',获取当前窗口的x，y位置。

 'get_window_size',获取当前窗口的宽度和高度。

 'implicitly_wait',用粘超时隐含等待被发现的元素， 或命令来完成。这种方法只需要调用每个会话一次

 'launch_app',

 'log_types',

 'maximize_window',最大限度地增加了的webdriver使用当前窗口

 'mobile',

 'name',返回此实例中的底层浏览器的名称。

 'orientation',获取设备的当前方位

 'page_source',获取当前页面的源代码。

 'quit', 关闭浏览器并关闭启动ChromeDriver时启动的ChromeDriver可执行

 'refresh', 刷新当前页面

 'save_screenshot',获取当前窗口的截图。如果返回FALSE 任何IO错误，否则返回True。使用完整路径在你的文件名。

 'service',

 'session_id',浏览器会话的字符串ID开始，这个控制的webdriver。

 'set_page_load_timeout',设置的时间量，以等待一个页面加载完成 之前抛出一个错误。

 'set_script_timeout',

 'set_window_position',设置当前窗口的x，y位置。 （window.moveTo）

 'set_window_size',设置当前窗口的宽度和高度。 （window.resizeTo）

 'start_client',开始一个新的会话之前调用。这种方法可能会被改写定义自定义启动行为。

 'start_session',创建具有所需功能的一个新的会话。

 'stop_client',执行quit命令后调用。这种方法可能会被改写定义自定义关机行为。

 'switch_to',

 'switch_to_active_element',

 'switch_to_alert',切换当前活动的alert窗口

 'switch_to_default_content',

 'switch_to_frame',切换至当前活动的frame元素

 'switch_to_window',切换至当前活动的浏览器窗口

 'title',返回当前页面的标题。

 'window_handles'返回当前会话中的所有窗口的句柄。

```

#### 推荐文档

```url
英文:
http://selenium-python.readthedocs.io/getting-started.html

中文，我参与了翻译过程:
http://selenium-python-zh.readthedocs.io/en/latest/
```

