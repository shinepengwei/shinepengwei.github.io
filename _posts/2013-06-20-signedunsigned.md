---
layout: post
title: 整数表示-signed和unsigned
category: "编程"
---

本文介绍了C语言中int数据类型的存储，以及signed和unsigned类型的区别和联系。

<h1>二进制补码编码</h1>

补码主要是为了在计算机中表示负数。对于无符号数来说，补码和二进制编码相同，但是对于有符号的整数来说，就有一定的区别。



在有符号的整数中，如果该整数为非负数，补码和二进制编码相同；但如果整数为负数，即第一个符号位为1，就和二进制编码有了非常大的区别。



对于一个正整数，假设有w位，我们写为<img src="http://www.codingart.info/wp-content/uploads/2013/06/062013_1410_signeduns1.png" alt="" />。如果是补码，则表示的真正的数为：



<img src="http://www.codingart.info/wp-content/uploads/2013/06/062013_1410_signeduns2.png" alt="" />



很容易理解，就是最高一位表示的是一个负数，其他的和正常二进制编码相同。



举个例子，一个字节8位的unsigned int型（即char）：



最大的值为：127，补码表示为：0X7F



最小的值为：-128，补码表示为：0X80



-1的补码表示为：0XFF



0的补码表示为：0X00

<h1>有符号数和无符号数的转换</h1>

C中也允许无符号数和有符号数的转换，<strong><span style="color: red;">原则是位表示保持不变</span>。

</strong>

<div>

<table style="border-collapse: collapse;" border="0"><colgroup> <col style="width: 738px;" /></colgroup>

<tbody valign="top">

<tr>

<td style="padding-left: 9px; padding-right: 9px; border: solid black 0.5pt;">注：有符号数和无符号数简单的转换容易出错，确切的说，有符号数的负数和无符号数的大于最大数值一般的数转换成对应类型时，就会出错！！</td>

</tr>

</tbody>

</table>

</div>

有符号数转换成无符号数的公式:



<img src="http://www.codingart.info/wp-content/uploads/2013/06/062013_1410_signeduns3.png" alt="" />



该公式表示对于非负整数，无需改变。对于复数，只要将最高位原本代表负数(<img src="http://www.codingart.info/wp-content/uploads/2013/06/062013_1410_signeduns4.png" alt="" />)改为正数，一负一正，成为<img src="http://www.codingart.info/wp-content/uploads/2013/06/062013_1410_signeduns5.png" alt="" />。



同理，将无符号数转换为有符号数的公式为：



<img src="http://www.codingart.info/wp-content/uploads/2013/06/062013_1410_signeduns6.png" alt="" />



C中有符号数和无符号数的转换就是基于以上公式进行的。



在C语言中，当执行一个运算时，<strong>如果一个运算数是有符号数另一个是无符号数，那C将隐含的将有符号参数转换为无符号参数，并假设他们为非负的。<span style="color: red;">

</span></strong>



如布尔运算-1&lt;0U，由于0后使用U表示该常量为无符号的，C也会将-1转换为无符号的，即一个可表示的最大的正整数。则返回false。

<h1>扩展数字的位</h1>

当位数较少的整数类型拓展为位数较多的整数类型时（有无符号不变），C语言中可以无差错转换拓展。



具体的拓展方法如下：

<ul>

	<li>将无符号数转换为更大的数据类型，只要在开头增加0.【零拓展】</li>

	<li>有符号数的拓展规则是：添加最高有效位（负数添加1）。【符号拓展】</li>

</ul>

<h1>截断数字</h1>

当将位数较多的数据类型转换为位数较少的数据类型（int-&gt;short），C语言会简单的将前面的数字截断。



很容易出错！

<h1>有符号数和无符号数的使用建议</h1>

有符号数和无符号数的转换过程中常常会出现问题，有一种方法是不引入无符号数，如JAVA。



但是，如果我们相对位进行操作时，无符号数是非常有用的。

<h1>关于signed和unsigned的题目：</h1>

转自http://blog.sina.com.cn/s/blog_8f6fade701011r4s.html



<span style="font-size: 9pt;">问题1：

</span>



<span style="font-size: 9pt;">int main()

</span>



<span style="font-size: 9pt;"> {

</span>



<span style="font-size: 9pt;"> char a[1000];

</span>



<span style="font-size: 9pt;"> int i;

</span>



<span style="font-size: 9pt;"> for(i=0; i&lt;1000; i++)

</span>



<span style="font-size: 9pt;"> {

</span>



<span style="font-size: 9pt;"> a[i] = -1-i;

</span>



<span style="font-size: 9pt;"> }

</span>



<span style="font-size: 9pt;"> printf("%d",strlen(a));

</span>



<span style="font-size: 9pt;"> return 0;

</span>



<span style="font-size: 9pt;">}

</span>



<span style="font-size: 9pt;"> 此题看上去真的很简单，但是却鲜有人答对。答案是255。别惊讶，我们先分析分析。

</span>



<span style="font-size: 9pt;"> for 循环内，当i 的值为0 时，a[0]的值为-1。关键就是-1 在内存里面如何存储。

</span>



<span style="font-size: 9pt;"> 我们知道在计算机系统中，数值一律用补码来表示（存储）。主要原因是使用补码，可以将符号位和其它位统一处理；同时，减法也可按加法来处理。另外，两个用补码表示的数相加时，如果最高位（符号位）有进位，则进位被舍弃。正数的补码与其原码一致；负数的补码：符号位为1，其余位为该数绝对值的原码按位取反，然后整个数加1。

</span>



<span style="font-size: 9pt;"> 按照负数补码的规则，可以知道-1 的补码为0xff，-2 的补码为0xfe……当i 的值为127时，a[127]的值为-128，而-128 是char 类型数据能表示的最小的负数。当i 继续增加，a[128]的值肯定不能是-129。因为这时候发生了溢出，-129 需要9 位才能存储下来，而char 类型数据只有8 位，所以最高位被丢弃。剩下的8 位是原来9 位补码的低8 位的值，即0x7f。当i 继续增加到255 的时候，-256 的补码的低8 位为0。然后当i 增加到256 时，-257 的补码的低8 位全为1，即低八位的补码为0xff，如此又开始一轮新的循环……

</span>



<span style="font-size: 9pt;"> 按照上面的分析，a[0]到a[254]里面的值都不为0，而a[255]的值为0。strlen 函数是计算字符串长度的，并不包含字符串最后的'\0 '。而判断一个字符串是否结束的标志就是看是否遇到'\0 '。如果遇到'\0 '，则认为本字符串结束。

</span>



<span style="font-size: 9pt;"> 分析到这里，strlen(a)的值为255 应该完全能理解了。这个问题的关键就是要明白char类型默认情况下是有符号的，其表示的值的范围为[-128,127]，超出这个范围的值会产生溢出。另外还要清楚的就是负数的补码怎么表示。弄明白了这两点，这个问题其实就很简单了

</span>



<span style="font-size: 9pt;">问题2：

</span>



<span style="font-size: 9pt;">int i = -20;

</span>



<span style="font-size: 9pt;">unsigned j = 10;

</span>



<span style="font-size: 9pt;">i+j 的值为多少？为什么？

</span>



<span style="font-size: 9pt;">解答1：int和unsigned int运算时int会自动转成unsigned int

</span>



<span style="font-size: 9pt;">int和unsigned int都是4字节（32位情况下）存储，区别是int最高位是符号位，用来表示正负

</span>



<span style="font-size: 9pt;">负数用补码存储，-20存储为11111111111111111111111111101100，这个东西转为unsigned int后就是一个很大的数4294967276了，所以最后结果是4294967286

</span>



<span style="font-size: 9pt;">解答2：这要看你的i+j的输出格式的 如果是%u 输出的话那肯定是没有负数的。应该是一个很大的正书。但是以%d输出的话就会转成int 类型了 就会是-10了

</span>



<span style="font-size: 9pt;">问题3：

</span>



<span style="font-size: 9pt;">下面的代码有什么问题？

</span>



<span style="font-size: 9pt;"> unsigned i ;

</span>



<span style="font-size: 9pt;"> for (i=9;i&gt;=0;i--)

</span>



<span style="font-size: 9pt;"> {

</span>



<span style="font-size: 9pt;"> printf("%u\n",i);

</span>



<span style="font-size: 9pt;"> }

</span>



<span style="font-size: 9pt;">解答：原因是 i&gt;=0 始终成立！

</span>



<span style="font-size: 9pt;">因为 i 被定义为无符号的整数。

</span>
