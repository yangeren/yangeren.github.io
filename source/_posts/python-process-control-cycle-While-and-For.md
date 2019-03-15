---
title: 循环（迭代器与生成器）的那点事
date: 2016-12-09 06:22:48
thumbnail: http://image.candymami.com/17-11-8/39347715.jpg?imageView2/1/w/640/h/320/format/webp/q/75|imageslim
tags:
- python
- 迭代
categories:
- python
permalink: python-process-control-cycle-While-and-For
---
循环（迭代器与生成器）的那点事
======

## while

- while 执行时的基本流程图

![image](http://image.candymami.com/while.png "while流程图")

<!--more-->

> while语句的基本格式

其中<exception>为对参数或表达式取布尔值，即：bool(<exception>)，结果只有两种，True和False

如下：

```python
while <exception>:
    <suite>
    <break>
else:
    <suite>
```

> tips:

讲述一下bool值：

1. 在数值上下文环境中，True被当作1, False被当作0；如True+2 => 3, int(True) => 1

2. 其他类型值转换bool值时，除了'', "", """", 0, (), [], {}, None, 0.0, 0L, 0.0+0.0j, False值为Faluse，其他的都为True；例如：bool(-1) => True



----

- while表达式中的break，指跳出最近的所在的循环；一般为满足条件后跳出，以防止死循环； 

举例：猜字游戏

```python
def guess():
    while True:
        input_seed = raw_input("please input the seed:")
        try:
            seed = int(input_seed)
            if seed == 11:
                print("congretulations to you!")
                break
            else:
                print("guess again!")
        except ValueError:
            print("please input int type")
        except Exception as e:
            print(e)
```

- else为可选部分，当控制权离开循环而又没有碰到break语句时，就会执行else中的语句；

[注意：当第一次判断while循环条件时就不满足时，即一次循环也没有执行时，还是会执行else语句的]

```python
def func1():
    keys = "goodjob"
    while keys:
        print(keys)
        keys = keys[1:]
    else:
        print("done")
```

```python
def func3():
    while False:
        print("false")
    else:
        print("the end")
```

- break, continue和pass

> break: 跳出最近所在的循环 （指跳出整个循环）

continue: 立即跳到循环的顶端，即跳过本次循环，执行下一次循环

pass: 占位语句，无实际作用

```python
a = 10
b = 0
while True:
    if a == 1: break
    b += 1
    print("b=",b)
    if b == 100: break
    if a == 3: continue
    a -= 1
    print("a=",a)
```

```python
def func2():
    while True:
        pass
```

----

## for

>for语句的基本格式和一般用法

```python
for <target> in <object>:
    <suite1>
else:
    <suite2>
```

>执行方式

当运行for循环时，会将序列对象中的元素逐个赋值给目标，然后为每个元素执行循环主体。



----

- for循环也有break, continue和else子句

```python
for <target> in <object>:
    <suite1>
    if <test>: break
    if <test>: contiune
else:
    <suite2>
```

- 基本用法 

> 求1-10之和，之积

```python
def func4():
    h = 0
    j = 1
    for x in xrange(1,11):
        h += x
        j *= x
    print h, j
```

- for中的元组赋值

若对应列表中的元组进行赋值，则可看做对元组的解包运算；

```python
def func5():
    T = [(1,2),(3,4),(5,6)]
    for x in T:
        print x
    for x, y in T:
        print(x,"=>",y)
```

- 结合items操作字典

for 循环中的元组使得用items方法来遍历字典中的键和值变得很方便，而不必再遍历键并手动索引以获取值

```python
def func6():
    D = {'a':1,'b':2,'c':3}
    for x in D:
        print x
    for x, y in D.items():
        print(x,"=>",y)
```

- 多重for循环

注意：else是第二层循环中的子句，而非if的子句

```python
def func7():
    items = ['aaa',111,(4,5),2.01]
    tests = [(4,5),3.14]
    for key in tests:
        for item in items:
            if key == item:
                print(key, "was found")
                break
        else:
            print(key, "not found")
```

- 常用range搭配，来做循环计数器和索引

> range，python2.7为生成list，python3.*与xrange合并，均变为生成可迭代的对象；

简单说区别：生成可迭代对象比生成list速度更快，占用内存更小，在大数据量情况下非常有效。

```python
list(range(-5,5))
>> [-5, -4, -3, -2, -1, 0, 1, 2, 3, 4]
list(range(5,-5,-2))
>> [5, 3, 1, -1, -3]
```

- 利用range进行索引，可以通过控制range来实现特殊的遍历，例如，可跳过一些元素

```python
S = 'abcdefghijklmn'
for i in range(0, len(S), 2): print S[i],
>> a c e g i k m
```

> 扩展：其他的跳过元素的方式，运用切片技术，如需要可详细了解：



```python
for item in S[::2]:print item,
>> a c e g i k m
```

- 并行遍历

对两组列表进行并行遍历

```python
def func9():
    a = [1,2,3,4]
    b = [11,22,33,44]
    c = zip(a,b)
    print type(c)
    print c
    for m, n in c:
        print m,"+",n,"=",m+n
>>
<type 'list'>
[(1, 11), (2, 22), (3, 33), (4, 44)]
1 + 11 = 12
2 + 22 = 24
3 + 33 = 36
4 + 44 = 48
```

> 扩展：zip和range一样，2.×时代生成为list，3.×时代均为可迭代对象；



---

- 产生偏移和元素: enumerate

有时可能会有这种需求，查看迭代的元素所在的位置，可以这样实现

```python

def func10():
    a = "abcdefg"
    j = 0
    for x in a:
        print x, "offset is ",j
        j += 1
>>>
a offset is  0
b offset is  1
c offset is  2
d offset is  3
e offset is  4
f offset is  5
g offset is  6
```

但内置函数enumerate可以帮我们做

```python
def func11():
    a = "abcdefg"
    j = 0
    for m, n in enumerate(a):
        print n, "offset is --", m
```

## 列表推导

> 列表推导式为从序列中创建列表提供了一个简单的方法。

### 普通创建列表方式
我们可以像这样创建一个普通的列表，像这样：
```python
testlist = []
for x in range(10):
    testlist.append(x**2)

print testlist

>>> [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

通过上面的程序可以发现，通过for循环中被创建的名为x的变量在循环完毕后，依然存在。
如果想完成上面的功能，且不会产生其他的副作用，可以使用lambda来创建：

### 通过lambda表达式来创建
```python
testlist = list(map(lambda x: x**2, range(10)))

>>> [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```
还有一种更加易读的创建方式：

### 通过列表推导创建

#### 基本列表推导式

```python
testlist = [x**2 for x in range(10)]

>>> [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```
这种方式更加易读，且创建起列表来得心应手。
下面我们来看看，它是如何来创建的，即它的格式是什么。

> 列表推导式由包含一个表达式的括号组成，表达式后面跟随一个 for 子句，
> 之后可以有零或多个 for 或 if 子句。结果是一个列表，
> 由表达式依据其后面的 for 和 if 子句上下文计算而来的结果构成。

等同于：
```python
testlist = []
for x in range(10):
    testlist.append(x**2)

testlist

>>>[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

#### 带控制流程的列表推导

来具体看个例子：
双重循环加判断：
```python
[(x, y) for x in [1,2,3] for y in [3,1,5] if x != y ]

>>> [(1, 3), (1, 5), (2, 3), (2, 1), (2, 5), (3, 1), (3, 5)]
```
如果看起来太复杂，那么变成常规的语法来看看：
上面的等同于：
```python
testlist = []
for x in [1,2,3]:
    for y in [3,1,5]:
        if x != y:
            testlist.append((x, y))

testlist

>>> [(1, 3), (1, 5), (2, 3), (2, 1), (2, 5), (3, 1), (3, 5)]
```
注意下上面的for 和if语句的顺序。

#### 嵌套的列表推导式

列表解析中的第一个表达式可以是任何表达式，包括列表解析。

再看一个例子：
有一个长度为4的列表组成的3×4的矩阵，如果想要交换行列，应如何操作？
```python
testlist = [
    [1,2,3,4],
    [5,6,7,8],
    [9,10,11,12],
]
```
##### 普通方式：
```python
result = []
for x in range(4):
    bb = []
    for y in testlist:
        bb.append(y[x])
    result.append(bb)

>>> [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
```

##### 一层列表推导：
```python
result = []
for x in range(4):
    result.append([row[i] for i in testlist])

result

>>> [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
```

##### 双层列表推导
```python
[[y[x] for y in testlist] for x in range(4)]
```

##### 内置函数处理
```python
list(zip(*testlist))
```
上次讲过一点zip的功能，这里的*号为参数列表的分拆，等同于：
```python
a = [1,2,3,4]
b = [5,6,7,8]
c = [9,10,11,12]
tt = zip(a,b,c)
result = list(tt)
```

### 练习
自己偿试将上次的food.py中的循环判断语句，使用列表表达式的方式进行处理


用自己的话来说一遍：
把最终要处理的表达式提在最前面，把循环语句放在后面；循环语句与判断语句按顺序依次排列。
每一层中扩号都执行了一次append操作。




## 文件迭代器

### 文件读取方法大集合

- 基本读取文件方法



- with的用法



## 生成器-generator

yield



## 迭代器
