---
layout: post
title:  csv导表
categories: 
- "编程"
- "项目"

tag: "C#"
---

工作中我们时常需要把特定的数据表通过代码生成器输出成可以方便访问的数据模块，我们称为导表。

请用C#编写一个代码生成器，从给定的csv数据文件生成一份python代码，使数据可以用dict进行组织，外部代码可以通过访问这个dict实现直接访问csv的数据

内容。

具体要求如下：

1.用C#编写一个代码生成器。

2.代码生成器需要从csv文件中读入表数据，请自己实现读入csv模块。csv文件如：

技能编号,技能名称,伤害,负面效果,消耗魔法值,字串1,字串2

10,含笑半步颠,150,,100,abc'def,abc\def

11,七日含包散,120,3,,"abc,def",

3.生成的代码为python，数据组织形式为dict，形式如：

```

# -*- coding: gb2312 -*-
data = {
10:{'技能名称':'含笑半步颠', '伤害':150, '消耗魔法值':100, '字串1':'abc\'def', '字串2':'abc\\def', },

11:{'技能名称':'七日含包散', '伤害':120, '负面效果':3, '字串1':'abc,def', },
}

```

4.提交一个名为Program.cs的源码文件，程序不依赖于windows和form窗体（推荐控制台程序）
包含入口函数static void Main(string[] args)
传入参数为两个，源csv文件路径和生成python文件路径，如

Program.exe c:\data.csv c:\data.py

5.csv数据有一个主key，在第一列，将作为dict的主索引。

6.请认真考虑不同数据类型的处理,自动识别整型和浮点型数据。

7.源csv文件和输出py文件均为gb2312编码，注意读写文件时的编码方式。

8.csv中为空的数据域不出现在python dict中。

9.注意生成python源码时字符串在python中的转义表示。

10.注意csv文件中特殊的域表示(如域中含有逗号等)。

11.当数据表有错导致无法转换时，请退出程序(exit code 2)并打印出表具体c出错的地方。

12.测试程序将尝试从生成的代码中读入data表中的内容，并判断是否正确。

##代码文档
代码：https://github.com/shinepengwei/CsvAnalyzer

由于csv格式不够统一，此项目根据RFC将csv格式定义如下：http://tools.ietf.org/html/rfc4180

- 每一条记录都是一个单独的行（除非字段中存在换行符），被换行符分割。
- 最后一条记录可以没有换行符
- 第一行可以作为头头记录，表示每条记录的每个字段相对应的名字。
- 在头记录和每条记录中，可以有多个字段，他们被逗号分割。最后一个字段不能有逗号。
- 每条记录的字段数量都相同。
- 空格可以包含在一个字段中，不能被忽略。
- 每个字段可以被双引号包含，如果字段没有被双引号包含，字段中不能出现双引号。
- 如果字段中包含换行符、双引号和逗号，那么该字段必须被双引号包含。
- 在一个被双引号包含的字段中，使用两个双引号表示该字段中存在一个双引号。



csv导表总的来说分为两个步：

- 解析csv文件，将数据存为C#中的字典格式。
- 将C#中的字典数据转为python数据格式的字符串，并将其输出到.py文件

代码构架为：
- 创建Analyzer类，存储字典的数据，同时以下两个模块的函数都属于这个类。
- 解析csv文件模块
- 输出为python字符串模块

##解析csv文件

在解析csv文件时，因为要考虑字段中含有换行符的情况，所以在对每条记录解析之前，先对每行字符串使用`isEnd()`进行判断，判断此行字符串是否代表着一条完整的记录，如果换行符前出现了一个双引号而没有另一个双引号结束，则说明这个换行符并不是代表着新的纪录，而是属于一个字段。

进行了以上处理以后，获得一个数组存储每条记录的字符串。然后对每条记录的字符串进行解析，主要涉及到以下函数：


- `getElementFromString(ref int curIndex, string str)`:解析输入文件的字符串str，根据当前解析指针curIndex，输出解析到的字符串，并将解析指针移动到下一个字符串开始部分。解析时要注意字段包含在双引号`"`中的情况，这时逗号、换行符属于字符串内的字符。

- `AnalyzeTitle(string titleStr)`: //解析第一行字符串，将其存到一个ArrayList中作为head，如'技能编号,技能名称,伤害,负面效果,消耗魔法值,字串1,字串2'

- `public void AnalyzeAContent(string contentStr)`:
//解析每一行记录的字符串，将其存到一个Dictionary中，其中key为cxv每一行第一列数据中的key，其他内容存在Dictionary中的value中，value的格式为ArrayList

##输出为python字符串模块
此模块将解析后的csv文件对应的存放在类中的数据转为python可读字符串，包括以下函数。

- `ConvertToPythonData()`:
将Dictionary数据根据要求转为python文件中的字符串。

- `ConvertContentLineToString(string key, ArrayList contentLine,ArrayList title)`：
将Dictionary中的某一个元素（key，value-contentLine<ArrayList>），输出为python字典data的一个元素。该函数用于`ConvertToPythonData()`

- `ConvertStringToPythonElement(string inStr)`：
将C#中的字符串inStr转为python中可识别的对象，主要要考虑:1.类型为int或float时直接输出。2.类型为string时前后要有单引号，同时，对于字符串中的每个字符，如果是`\ ' "`，需要加上转义字符。此函数用于`ConvertContentLineToString`函数。


