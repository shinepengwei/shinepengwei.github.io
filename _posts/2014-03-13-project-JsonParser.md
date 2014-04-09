---
layout: post
title:  JsonParser - Python json转换
date:   2014-03-12 13:46:11
categories: 
- "编程"
- "项目"
tag: "Python"
---

基于Python 2.5.X封装实现一个可重用的类，具体要求为:

1、该类能读取json格式的数据，并以Python字典的方式读写数据

2、给定一个Python字典，可以更新类中数据，并以json格式输出

3、遵循json格式定义确保相同的同构数据源彼此转换后数据仍然一致；

4、支持将数据分别以json或Python的格式存储到文件并加载回来使用；

5、只允许使用Python内置模块(如math,sys)及标准模块string，不允许使用其他标准模块(如re)，不允许使用任何第三方开发库，包括但不限于simplejson。

6、独立完成此作业，并附带测试代码及一份简短的模块使用说明

###基本要求：
提交python代码文件名为JsonParser.py，测试代码和文档（如果有）可以打一个包作为第二个附件提交
JsonParser.py中包含一个class，类名为JsonParser，JsonParser类中包含以下方法：

1、load(self, s)，读取json格式数据，输入s为一个json字符串，无返回值

若遇到json格式错误的应该抛出异常，json中数字如果超过了python里的浮点数上限的，也可以抛出异常

所有字符串（键和值）转化为python中的unicode类型字符串，特别注意转义符的转化

为简便考虑，json的最外层假定只为object

2、dump(self)，根据类中数据返回json字符串

3、loadJson(self, f)从文件中读入json格式数据，f为文件路径，异常处理同1，文件操作失败抛出异常

4、dumpJson(self, f)将类中的内容以json格式存入文件，文件若存在则覆盖，文件操作失败抛出异常

5、loadDict(self, d)，读取dict中的数据，存入类中，若遇到不是字符串的key则忽略

6、dumpDict(self)，返回一个字典，包含类中数据

做5和6的操作时，注意不要直接引用输入参数d，也不要返回类中内置的字典的引用

###进阶要求：

JsonParser类支持用[]进行赋值、读写数据的操作，类似字典

JsonParser类包含方法update(self, d)用字典d更新类中的数据，类似于字典的update

其实实现了进阶要求JsonParser才能成为一个真正有意义的可读写修改的json interface，否则只是一个包含很多静态方法的dict-json转换器

基本测试方法：

```
a1 = JsonParser()
a2 = JsonParser()
a3 = JsonParser()

#test_json_str、test_dict
a1.load(test_json_str)
d1 = a1.dumpDict()
#粗糙比较test_dict和d1

a2.loadDict(d1)
a2.dumpJson(file_path)
a3.loadJson(file_path)
d3 = a3.dumpDict()
#比较d1和d3是否完全一样
```

##代码文档

代码：
https://github.com/shinepengwei/python/blob/master/JsonParser.py


总的来说，该类实现两个基本功能：

1 解析Json字符串转为Python对象保存在JsonParser中的字典中（内存里面），
2 将JsonParser字典中的对象转为Json字符串


**解析Json字符串并将其保存到JsonParser类中的字典对象中的过程：**

输入字符串--（python读取字符串）-->Python中的字符串
--（Python解析Json字符串）-->Python对象


第一个过程包括Python的自动转移，如果使用原始字符串类型r，则第一步不需要考虑。


**将JsonParser字典中的对象转为Json字符串的过程：**

Python对象--（Python对象转为Json字符串）-->Json字符串


————————————————————————————————————————————————————————————————————————————


代码主要分为以下模块：

- 字符串转为python中的对象
- 将python中的对象转为j相应的son字符串
- 深拷贝


此外，为实现python对象和json字符串的转意，特别需要注意两个问题：
1. 编码问题，即Unicode与其他编码的转换，以及pythong中对中文的处理
2. 转意字符的处理


另外还有一些对象的[]读写更新操作，使用的`__setitem__,__getitem__,update`函数，比较简单，就不进行深入介绍

##字符串转为Python对象
以下函数的功能是处理字符串，并且将字符串前一段转为相应的Python对象，忽略剩下的字符，返回转换到的相应Python对象，并且返回目前执行到的字符串偏移量。


如字符串`'"aaa"{“b”:1}'`，函数`getString`处理字符串`'"aaa"'`，忽略剩下的留给后面继续解析，并且返回index告诉后面的函数目前已经解析到何处。

函数在解析字符串过程中，会忽略Json元素之间的空格、制表符和换行符。

该模块包括以下函数：

- getString(str)：解析字符串str，返回Python字符串对象和解析字符串的偏移量。
- getObject(str):返回Python映射类型（字典）对象。对应Json中的对象元素。
- getArray(str):返回Python中的数组对象。
- getNumber(str):返回Python中的数字对象，包括浮点数和整数。
- getKeywords(str):返回Python中的关键字，包括：`True,False,None`
- getValue(str):解析Json中的value元素，根据第一个有效字符（即不为空格、制表符和换行符的字符）判断这个value元素具体是什么类型，然后调用相应的函数进行解析。


##将Python中的对象转为Json相应的字符串
该模块函数将Python的对象转为Json字符串，转换过程中根据对象的类型递归调用相应的函数。

如dict对象的元素value值可能是数组，数组的元素又可能是字典，因此是递归调用，直到string、number等对象。

该模块包括以下函数：

- convertStringToJson(s):将Python的字符串对象转为Json字符串，包括加上`"`以及处理转义字符。
- convertListToJson(l):将List对象转为Json中的数组。[,]
- convertDictToJson(d):将dict对象转为Json中的对象。{,}
- convertValueToJson(v):将当前对象转为Json中的value，判断当前对象的类型，调用相应的函数进行转换。

##深拷贝
深拷贝只需要处理容器类型，包括数组和字典。
http://www.codingart.info/python-storage/

设计递归函数deepCopy(v)，如果v为非容器对象，直接返回v。

如果v为数组或者字典，针对每个数组元素和字典的value，递归调用深拷贝。

## 编码问题
为了解决Python中的中文编码问题以及在dict对象内以Unicode方式存储字符串，使用了以下方法解决编码问题：

- 使用utf-8作为默认的编码格式，代码以utf-8编码保存。
- 在Json字符串解析前，先对Json字符串使用utf-8解码为Unicode。
- 在Json字符串解析过程中，生成的Python字符串对象为Unicode类型。
- 在写文件操作时，将Unicode字符串编码为utf-8格式保存。
- 读取的文挡默认为使用utf-8保存，读取了文档中的字符串然后使用utf-8解码。


通过以上方法，解决了中文字符串问题，并且保存在对象中的字符串为Unicode格式。

总的来说，目前对编码问题理解的还是不是很透彻，有待下一步继续了解。

##转意字符的处理
转义字符的处理分为两块，第一块为Json字符串解析为Python对象时如何处理，另一块为Python对象转为Json字符串是转义字符的处理。

第二块相对简单，只要遇到了Json中的转义字符，就转为Json字符串的相应格式。定义了`charToZhuanYi={'\"':'"','\\':'\\','\b':'b','\f':'f','\n':'n','\r':'r','\t':'t'}`，遍历Python字符串时遇到了相应的转意字符，就改为一个`\`加上相应的字母即可。

而将Json字符串解析为Python对象是的处理稍微麻烦一点。

首先，Json字符串输入到Python时Python会首先对字符串里面的转义字符进行处理，所以如果不是原始字符串，那么就需要考虑这种问题，所以输入的字符串是Python首先转移处理，处理后的字符串才是Json字符串。

然后，当遇到转义符号'\'时，就根据后面的字母生成转义字符。

但是，遇到\u时需要额外处理，因此这是代表着Unicode编码格式，`\uXXXX`表示一个Unicode编码字符，保存到Unicode里面是一个字符，所以当遇到\u时，使用eval特殊处理。

——————————————————————————————————————————————————————————————————————————————————————————

另外，2.5和2.7还是有一定的区别的，据说是set和float('inf')，但是我在2.7使用了inf依然通过测试。

版本问题我并没有深究。







