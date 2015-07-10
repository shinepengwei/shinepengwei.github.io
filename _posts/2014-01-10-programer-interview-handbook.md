---
layout: post
title:  程序员面试宝典
category: "理论及算法"
tag: "读书笔记"
---

十进制转N进制
<div>十进制整数转换N进制整数：<strong>除N取余，逆序排列</strong>。用N整除十进制整数，可以得到一个商和余数，在用N去除商，又获得一个商和余数，如此进行，直至商为0位置，然后把前面获得的余数作为低位，后面的余数作为高位排列。</div>
<div>十进制小数转换为N进制小数：<strong>乘N取整，顺序排列</strong>。用N成十进制小数，可以得道积，积的整数部分取出，然后用N乘剩下的小数部分，又获得一个积，然后将整数取出。以此类推，直至积中小数为0.然后把取出的整数部分按顺序排列起来，先取的整数作为二进制小数的高位有效位，后取的整数作为低位有效位</div>
<div>

<hr />

在布尔表达式中，A &amp;&amp; B和A || B 按顺序判断前后的表达式，如果A为false，则&amp;&amp;后面的B不执行，因为无论执行不执行总体表达式不可能为true。而如果A为true，||后面的B也不执行

</div>

<hr />

C中printf计算参数时是从右到左压栈的。如printf("%d,%d\n",*ptr,*(++ptr));语句先执行后一个语句再执行前一个。
<div>

<hr />

用一个表达式判断X是否是2^N次方，不用循环语句：!(x&amp;(x-1))

</div>
<div>

<hr />

宏定义：

</div>
<div>#define SECOND_PER_YEAR (365*24*60) UL</div>
<div>#define MIN(a,b) ((a)&gt;(b)?(b):(a))</div>
<div>宏中要把参数用括号括起来</div>
<div>

<hr />

在C++中，如果类中的数据成员加上mutable，修饰为const的成员函数就能对他进行修改

</div>
<div>

<hr />

</div>
<div>sizeof</div>
<div>char：1</div>
<div>数组：根据数组元素和类型，sizeof(元素)*数量</div>
<div>指针：无论是啥指针，一定是4</div>
<div>结构体：结构体的长度一定是最长数据元素的整数倍，因为要考虑字节对齐，这是为了方便对结构体内元素的访问和管理。</div>
<div>sizeof(string):4</div>
<div>空类：1</div>
<div>单一继承和多重继承的空类也为1</div>
<div>但是涉及到虚继承和虚函数时，类中多了一个四字节的虚指针</div>
<div>

<hr />

sizeof和strlen的区别

</div>
<div>sizeof求字节数，可以使变量也可以使类型。还可以以函数作为参数，即计算函数返回值的size。</div>
<div>strlen的参数只能是char*，而且必须是以‘\0’结束。</div>
<div>

<hr />

指针的减运算：

</div>
<div>int *p=..,*q=..</div>
<div>p-q=(p的地址值-q的地址值)/sizeof(int)</div>
<div>

<hr />

</div>
<div>main函数不写return语句，编译器隐式返回0.</div>
<div>

<hr />

对于既未使用任何成员数据也不是虚函数的函数，不需要this指针，也不需要动态绑定，即使一个指向NULL的对象指针调用这种函数也可以正常运行。

</div>
<div>

<hr />

在windows平台，栈都是从上向下生长的，即先入栈的地址更高。

</div>
<div>在调用函数时候，先按照从右向左的方向把参数压入栈，然后把函数地址压入栈。</div>
<div>

<hr />

char值的取值范围为-128~127

</div>
<div></div>
<div></div>