---
layout: post
title: 数组和指针
category: "编程"
tag: "C++"
---
<p>数组和指针在一定程度上可以互相使用，但是从本质上来说，他们是有本质的区别的。
</p><h2>X=Y的本质
</h2><p>介绍数组和指针之前，首先说一下X=Y这个简单的赋值语句的本质。
</p><p>对于一个变量，如X或者Y，他是有两种含义的，一是代表着是X的那一块内存空间，二则达标者X的内容。
</p><p>对于X=Y，左边的X代表的是X的那一块内存空间，而Y代表着Y的内容，可以理解为：将Y变量的内存空间里面的内容复制到X变量的内存空间里面去。
</p><p>更加专业的角度来说，赋值符的左边成为左值，右边成为右值。编译器为每一个变量都分配一个地址（<strong>左值</strong>），这个地址是编译器知道的，而且这个变量和这个地址是永久对应不会改变的。也可以说：对于编译器来说，一旦为变量分配了地址，<strong>变量名就是这个地址</strong>。存储在这个地址的内容（右值）只有在运行的时候才是可以知道的。
</p><p>综上所述：<strong>变量的地址永远不变，变量的内容在运行时可以改变，我们对变量的修改操作对象其实就是变量的内容。
</strong></p><p>
?</p><h2>数组和指针的访问方式
</h2><p>每个变量（数组和指针）的地址都是编译器可知的，因此数组的存储地址编译器是知道的，每次访问第N（常数）个元素的时候编译器可以直接计算出来这个元素的所在地址。而对于指针来说，编译器虽然知道这个指针变量的存储地址，但是他不知道这个指针变量的内容，及指针指向的地址，所以如果要通过指针访问第N个元素，需要在运行的时候获得指针指向的地址，然后通过计算获得该元素的最终地址。
</p><p><strong>举例：数组的访问方式
</strong></p><p>Char a[5] ="abcd";
</p><p>Char c=a[i];
</p><p>假如，编译器为数组a分配了5个字节的内存，首地址为9980。
</p><p>运行时：
</p><p>步骤1：取i的值，并将其和9980相加。
</p><p>步骤2：取（9980+i）的内容。
</p><p><strong>两步读内存过程</strong>。
</p><p><strong>举例：指针的访问方式
</strong></p><p>char *p;
</p><p>…
</p><p>Char c=p[i];
</p><p>假定，编译器为指针变量p分配了地址4624，地址大小为四个字节（32位）。编译器并不知道这个指针指向哪里，因为他不知道里面存储的内容，需要在运行时才知道。
</p><p>运行时
</p><p>步骤1：取变量p的值，即地址为4624的内容，假定取出来后为9980.
</p><p>步骤2：取i的值，将其与9980相加。
</p><p>步骤3：取地址为（9980+i）的内容。
</p><p><strong>三步读内存过程。
</strong></p><h2>数组和指针的区别
</h2><div><table style="border-collapse:collapse" border="0"><colgroup><col style="width:355px"/><col style="width:355px"/></colgroup><tbody valign="top"><tr><td style="padding-left: 9px; padding-right: 9px; border-top:  solid black 0.5pt; border-left:  solid black 0.5pt; border-bottom:  solid black 0.5pt; border-right:  solid black 0.5pt"><p><span style="font-size:10pt">指针</span></p></td><td style="padding-left: 9px; padding-right: 9px; border-top:  solid black 0.5pt; border-left:  none; border-bottom:  solid black 0.5pt; border-right:  solid black 0.5pt"><p><span style="font-size:10pt">数组</span></p></td></tr><tr><td style="padding-left: 9px; padding-right: 9px; border-top:  none; border-left:  solid black 0.5pt; border-bottom:  solid black 0.5pt; border-right:  solid black 0.5pt"><p><span style="font-size:10pt">保存数据的地址</span></p></td><td style="padding-left: 9px; padding-right: 9px; border-top:  none; border-left:  none; border-bottom:  solid black 0.5pt; border-right:  solid black 0.5pt"><p><span style="font-size:10pt">直接保存数据</span></p></td></tr><tr><td style="padding-left: 9px; padding-right: 9px; border-top:  none; border-left:  solid black 0.5pt; border-bottom:  solid black 0.5pt; border-right:  solid black 0.5pt"><p><span style="font-size:10pt">间接访问数据。首先取指针内容，然后将其作为地址再去取数据。</span></p></td><td style="padding-left: 9px; padding-right: 9px; border-top:  none; border-left:  none; border-bottom:  solid black 0.5pt; border-right:  solid black 0.5pt"><p><span style="font-size:10pt">直接访问数据。根据数组的地址。</span></p></td></tr><tr><td style="padding-left: 9px; padding-right: 9px; border-top:  none; border-left:  solid black 0.5pt; border-bottom:  solid black 0.5pt; border-right:  solid black 0.5pt"><p><span style="font-size:10pt">通常用于动态数据结构-堆，当然，指向数组也可以。</span></p></td><td style="padding-left: 9px; padding-right: 9px; border-top:  none; border-left:  none; border-bottom:  solid black 0.5pt; border-right:  solid black 0.5pt"><p><span style="font-size:10pt">存储固定数目且数据类型相同的数据，一般是放在栈里面作为临时变量。</span></p></td></tr><tr><td style="padding-left: 9px; padding-right: 9px; border-top:  none; border-left:  solid black 0.5pt; border-bottom:  solid black 0.5pt; border-right:  solid black 0.5pt"><p><span style="font-size:10pt">使用malloc、free为其分配内存。</span></p></td><td style="padding-left: 9px; padding-right: 9px; border-top:  none; border-left:  none; border-bottom:  solid black 0.5pt; border-right:  solid black 0.5pt"><p><span style="font-size:10pt">隐式分配、删除</span></p></td></tr><tr><td style="padding-left: 9px; padding-right: 9px; border-top:  none; border-left:  solid black 0.5pt; border-bottom:  solid black 0.5pt; border-right:  solid black 0.5pt"><p><span style="font-size:10pt">指向匿名数据</span></p></td><td style="padding-left: 9px; padding-right: 9px; border-top:  none; border-left:  none; border-bottom:  solid black 0.5pt; border-right:  solid black 0.5pt"><p><span style="font-size:10pt">本身就是数据，数组名就是数据的名字</span></p></td></tr></tbody></table></div><p>
</p><p>数组和指针在定义时都可以用字符串常量对其初始化，但是底层机制不同。
</p><p>定义指针时，编译器为指针分配空间，创建的字符串常量是只读的，并将其地址存放在指针变量中，因此不能对其进行修改。
</p><p>而通过字符串常量初始化数组，会将每个字符存入到编译器为数组分配的内存空间里面，也就是可以修改的。
</p><h2>数组和指针的使用
</h2><p>声明：
</p><ul><li><div style="text-align: justify">extern char a[];不能修改为指针的形式。
</div></li><li><div style="text-align: justify">定义：char a[10];不能修改为指针形式。
</div></li><li><div style="text-align: justify">函数参数：f(char a[])；数组形式或指针形式都可以
</div></li></ul><p>表达式中的使用：
</p><ul><li><div style="text-align: justify">如c=a[i];数组或指针形式都可以。
</div></li></ul>
<h2>数组和指针的可交换性
</h2><p>什么时候数组和指针是相同的，C语言标准中做出了一下的说明：
</p><ul><li><div style="text-align: justify">表达式中的数组名被编译器当作一个指向该数组第一个元素的指针。
</div></li><li><div style="text-align: justify">C语言把下标作为指针的偏移量。
</div></li><li><div style="text-align: justify">作为函数参数的数组名等同于指针。
</div></li></ul><p>可交换性的总结：
</p><ul><li><div style="text-align: justify">用a[i]的形式访问数组，将被编译器解释为*(a+i)这样的指针访问。
</div></li><li><div style="text-align: justify">指针始终为指针，他不可能改写为数组。下标形式访问指针，也是说明指针指向的是一个数组，编译器也将其解释为*(a+i)这样的形式。
</div></li><li><div style="text-align: justify">在将数组声明为函数的参数时，可以将其看成一个指针，他也具有一个内存空间，也可以修改（数组名没有内存空间也不能修改）。
</div></li><li><div style="text-align: justify">因此，定义函数参数时，指针和数组对边一起来说是一样的，函数内部其实都是一个指针。<br/>在其他情况，定义和声明必须匹配。定义一个数组，那在其他文件对其声明也必须声明为数组。
</div></li></ul>
