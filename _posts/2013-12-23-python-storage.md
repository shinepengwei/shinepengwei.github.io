---
layout: post
title: Python中的浅拷贝和深拷贝
category: "编程"
tag: "Python"
---

在*Python语法基础*中已经介绍了Python的内存管理方式，Python的赋值其实是简单的对象引用，同时通过对象计数的方式对对象进行管理。由此可以，当对一个对象进行拷贝到的时候，其实就是进行了简单的引用和计数加一。
##浅拷贝实例

例如创建一个person对象，然后将这个对象为一对夫妻拷贝一份（使用切片操作和工厂方法）：


```
>>>person=['name',['savings',100]]
>>>hubby=person[:]
>>>wifey=list(person)
>>>[id(x) for x in person,hubby,wifey]
[102231,102423,102333]

```


由上述结果可知，person、hubby和wifey分别为三个不同的对象，他们的内存管理方式应该是这样的：
![](/images/1.png)


对夫妻双方信息进行更新：

```
>>>hubby[0]='joe'
>>>wifey[0]='jane'
>>>hubby,wifey
(['joe',['savings',100]],['jane',['savings',100]])

```

现在的内存应该是这样的：
![](/images/2.png)


现在对存款帐号进行更新：

```
>>>hubby[1][1]=50
>>>hubby,wifey
(['joe',['savings',50]],['jane',['savings',50]])
```

只对丈夫的账号进行了更新，却发现妻子的账号也更新了，这是因为我们只使用了浅拷贝，对于第二个列表元素（存款），其实丈夫和妻子对象指向的是一个对象。那为什么第一个元素（名字）修改没有影响？那是因为第一个元素是不可变的，而第二个时刻变得。

这时他们的内存对象应该是这样的：
![](/images/3.png)


##深拷贝方法
如何实现深拷贝：copy.deepcopy()函数

```

>>>person=['name',['savings',100]]
>>>hubby=person
>>>import copy
>>>wifey=copy.deepcopy(person)
>>>wifey=list(person)
>>>hubby[0]='joe'
>>>wifey[0]='jane'
>>>hubby,wifey
(['joe',['savings',100]],['jane',['savings',100]])
>>>hubby[1][1]=50
>>>hubby,wifey
(['joe',['savings',50]],['jane',['savings',100]])

```

注意：

- 非容器类型（如数字、字符串和其他原子类型对象）没有被拷贝一说，
- 如果元组变量只包含原子类型对象，对他进行深拷贝将不会被进行。比如将上述例子中的账户信息改成元组，那就不可以被深拷贝。








