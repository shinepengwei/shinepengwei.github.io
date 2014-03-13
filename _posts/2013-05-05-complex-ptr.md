---
layout: post
title: 复杂指针声明解析
category: "编程"
tag: "C++"
status: publish
type: post
published: true
meta:
  _edit_last: '1'
  _wp_old_slug: ! '%e5%a4%8d%e6%9d%82%e6%8c%87%e9%92%88%e5%a3%b0%e6%98%8e%e8%a7%a3%e6%9e%90'
---
指针是C和C++非常常用的东东，但是其复杂性也是非常突出的特点。比如指针的定义，指针可以指向各种元素，比如基本变量、对象、数组或者函数，同时函数返回也可以使指针，数组的元素也可以使指针。因此，有时候指针的定义就很晦涩。一般来说，非常复杂的指针定义在真实的编码时尽量减少使用，但是如果遇到了非常复杂的指针声明，我们也得明白、看得懂。这在一些场合比如找工作，是非常重要的。

如何定义一个指针，或者对于一个复杂定义的指针，我们如何去理解。在《thinking in C++》作者提出了一个方法，我觉得是最易懂也是很有用的。

<strong>Start in the middle and work your way out</strong>:从中间开始找到声明的指针名，然后通过右、左、右、左的方式审查完所有的元素。

<img src="http://www.codingart.info/wp-content/uploads/2013/05/050513_0739_1.png" alt="" />

下面举一些例子：

void (*funcPtr)();

这个指针可以这样理解：先从中间开始，找到funcPtr是指针名（funPtr是一个）。然后开始找到我们出去的路：funcPtr右边是括号，因此去左边，发现左边是*，则说明这是一个指针（指针，指向…），朝右看是左右括号并且没有参数列表（一个没有参数的函数），再朝左看，发现是一个void（这个函数返回值为void）。通过以上过程，我们就理解了funcPtr是一个指针，指向一个没有参数的函数，这个函数的返回值为void。

如果没有括号：void *funcPtr();

这是一个普通的函数声明。如果使用我们的方法来分析则是：找到funcPtr朝右，说明funcPtr是一个函数，朝左void*，这个函数返回void*。说明这个方法不仅适合指针的声明，也适合其他元素的声明。

<img src="http://www.codingart.info/wp-content/uploads/2013/05/050513_0739_2.png" alt="" />

下面是数组指针和指针数组的例子：

float (**def)[10];

找到def，右看为括号，左看**说明是一个指针A的指针。右看[10]，说明指针A指向一个长度为10的数组，朝左看数组的元素为float。所以，def是一个二级指针，它指向一个指向长度为10元素为float数组的指针。

Double* (*def)[10];

Def是一个指针，它指向一个长度为10的数组，数组里面存放的是double*元素。

Double(*f[10])();

F是一个数组，数组里面存放的是指针，这些指针是指向无参返回void函数的指针。

<img src="http://www.codingart.info/wp-content/uploads/2013/05/050513_0739_3.png" alt="" />

来一个复杂点的例子：

Typedef double (*(*(*fp)())[10] ) ();

fp a;

首先,fp是一个函数指针，这个函数没有参数，返回的是一个指针，这个指针指向一个长度为10的数组，数组里存放的是指针，这个指针是一个无参返回值为double的函数指针。

英文是：An fp is a pointer to a function that takes no arguments and returns a pointer to an array of 10 pointers to functions that take no arguments and return doubles.

嗯，这局英语真是复合句的典型….

注意，我们将这个坑爹的指针通过typedef定义为fp，后面就可以使用fp定义a。

这么复杂的例子我感觉如果谁在现实的开发过程中用到了，谁就要考虑下你写的代码能不能通过审查，你会不会被老板骂了，不过这么复杂的如果都可以理解的话，其他应该就是小case了。
