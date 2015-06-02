---
layout: post
title:  使用snapshot检测Lua中的内存泄露
category: "游戏研发"
tag:  "IOS"
---


本文使用云风大大写的内存泄露检测工具：http://blog.codingnow.com/2012/12/lua_snapshot.html，主要介绍此工具如何使用。

###1 使用mingw编译dll文件

mingw的安装方法：http://tieba.baidu.com/p/2826016538

GCC的使用方法：http://www.cnblogs.com/benio/archive/2010/10/25/1860394.html

![Alt text](/images/2015/1426694222116.png)

要把mingw的path放在lua的系统路径之前，因为有一个dll文件重名。

我编译好的snapshot.dll文件可供大家下载：http://pan.baidu.com/s/1GsXIY (使用的话需要有mingw的一个dll文件)

###2 Lua的内存泄露原理

Lua中的内存泄露和传统如C++的内存泄露本质不一样，Lua有垃圾回收，所以其实本质上并没有内存泄露，GC从根部开始扫描，只要这个对象还在这个树引用中，就不会release这块对象的内存。

那么，Lua中的内存泄露其实代表着某些Lua对象仍然在全局的引用树中，但是以后并不会被使用了。

此外，循环引用在Lua的GC中是可以被释放的，如A引用了B，B引用了A，如果仅仅是根据引用关系来看，A和B都是被引用的，但是Lua的GC原理是只便利在全局树下的引用关系，所以A和B是可以被释放的。

游戏逻辑层中的内存泄露：http://colen.iteye.com/blog/588897

#####检测原理：
（转自西山居 QA 团队http://qa.xishanju.com/?p=234）

lua中支持垃圾回收机制的对象有五种：string，table，function，full userdata，thread。而他们的引用直接或间接的保存到：lua_state对象，_G全局表，Registry注册表，global_state->mt中。

 在脚本中：
- 运行的lua脚本本身就是lua_state。
- _G就是_G全局表。
- Registry表可以用debug.getregistry获取。
- global_mt可以用debug.getmetatable获取。

 所以我们就可以在脚本层次实现内存泄露的检测模块。

 在搜索时需要注意的几点：
 1. table 额外搜索metatable，若metatable中的__mode取值为”k"、"v"或者”kv"需特殊处理（补充中有说明）
 2. function 额外搜索 enviroment，也是一个table。额外搜索upvalues，这个可以是任何类型。
 3. 由于userdata在script层次不能被修改，所以搜搜他的metatable吧
 4. thread对象就是coroutine对象，在script中一般都不会创建多个coroutine，所以在脚本中没搜索它。若是需求的话，获取到它的线程函数，然后再按照第2步操作就可以了。

![Alt text](/images/2015/1426944320477.png)

###3 Cocos中使用方法

在cocos中使用，使用local snapshot = package.loadlib("snapshot.dll", "luaopen_snapshot")()而不是require。

require和package.loadlib有一定的区别，但是在cocos中使用require找不到，也不知道为什么。

区别：http://blog.csdn.net/sozell/article/details/5070223

#####快照的位置

在cocos中一般检测一个场景是否产生了内存泄露，比如从场景Start中进入场景A，然后从场景A又返回到场景Start，我们希望检测场景A中有没有出现内存泄露，可以使用以下方式：
- 在场景Start记录快照1
- 场景A返回到场景Start之前，记录快照2
- 返回场景Start后记录快照3，用3和1、2比较，如果在快照3中存在在快照1中不存在且在快照2中存在的内容就是资源泄漏。

解释：
- 快照3中存在在快照1中不存在：表示在场景A中或者返回场景Start后新创建的内容
- 且在快照2中存在：因为第一次进入场景Start和返回场景Start在内存中这是两个场景，因此要将返回场景Start后新建的内容去掉。

为何不在快照2记录的时候直接显示内存泄露？因为这时候场景A中创建的内存Lua对象还在内存中。快照3中依然还存在的场景A中创建的Lua对象才是真正的内存泄露。


###4 snapshot解析
云风大大知识介绍了[snapshot的基本原理](http://blog.codingnow.com/2012/12/lua_snapshot.html)，但是对于新手来说，看一下程序会有更深的理解。

**程序思路：**

深度优先遍历全局变量G的引用树对于访问到的每个对象存在dL[MARKED]以及dl[TYPE]中，

其中MARKED保存该对象是否被访问过，dl[TYPE]保存该对象的引用信息。

dl[TYPE]的类型为一个table，key = 对象的lightuserdata即指针值，value为一个记录它引用关系table。

记录引用关系table的key = parent的指针值，value为parent的描述descript

遍历阶段的操作在mark_系列函数中。

保存信息在函数readobject中。

最后将dl[]中的各类对象的应用信息输出。

遍历去全局变量树的时候，需要考虑弱表、元表、函数的upValue。

以mark_table函数为例，此函数对一个talbe进行遍历。

首先判断该table是否为弱表，然后递归调用mark_table(table的元表)，然后基于是否是弱表，处理表中的每个元素。



###5 另外一种内存泄露检测的方式
所以我们可以用另外一种实现：就是把用户自己创建的资源对象统统都丢到weak表中，运行完程序后强制GC，然后去查看weak表，若表中还保存着那个对象，就意味着这个对象还有外部引用（相对弱引用我们就叫它为强引用吧），资源没有被GC掉，所以我们可以说这个对象很有可能是内存泄露了。


###Reference

Step By Step(Lua调用C函数)  http://www.cnblogs.com/stephen-liu74/archive/2012/07/23/2469902.html

Lua脚本调用C++动态库 http://blog.csdn.net/sozell/article/details/5070223   
