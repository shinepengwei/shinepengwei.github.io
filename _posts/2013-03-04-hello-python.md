---
layout: post
title: Hello Python!
category: "编程"
tag: "Python"
---
本文介绍了我写的第一个python程序学习的相关模块内容，记录我编写的第一个python程序所学到的知识，只要包括基本语法和字符串、文件和时间的常见操作。
<h1>Python环境</h1>
下载的地址是：

<a href="http://www.python.org">www.python.org</a>

运行环境可以是linux或者是windows：
1、linux
redhat的linux安装上去之后一定会有python的（必须的组件），在命令行中输入python回车。这样就可以进入一个
&gt;&gt;&gt;的提示符

2、windows

安装好了python之后，在开始菜单里面找到Python2.3-&gt;IDLE，运行也会进入一个有&gt;&gt;&gt;提示符的窗口
<h1>基本语法</h1>
Python中，if/elif/else/for…in/while后面都是使用冒号":"，使用缩进而不是花括号表示判断或循环体。

常用的还有：

布尔值为True和False（首字母大写）

in ：判断一个元素是否在一个集合（元组、列表）中，返回Ture或False

is：判断两个变量是否指向同一地址，==比较变量是否相等

del：删除元素（del a[0]）

break/continue和C中一样。
<h1>字符串操作</h1>
复制字符串=

连接字符串+=

查找字符串：str.index(str2)

比较字符串cmp(str1,str2)

扫描字符串是否包含指定字符串str1 in str2

小写转换大写：str.upper()

追加制定长度：str1+=str2[0:n]

分割字符串str.split('str2')
<h1>文件操作</h1>
打开文件

[py]

f=open(&quot;d:\\1.txt&quot;,'r')

[/py]

或：

[py]
out=file(&quot;d:\\2.txt&quot;,&quot;w&quot;)
[/py]

好像两种方法都一样。

然后读文件有：

[py]
con=f.read()#读文件

con=f.readlines()#按行读文件，读出来的文件是一个数组，每行为一个元素

con=f.readline()#读一行文件，并将光标下移一行
[/py]

写文件

[py]
f.write(content)

f.close()
[/py]
<h1>时间操作</h1>
时间可以表示为Unix格式的时间值（1970.1.1到现在经过的秒数），或者一个时间的结构

使用time模块前要import time

获得当前时间值：time.time()

格式化输出时间：time.strftime("%y/%m/%d",now)

将字符串转化为时间对象：strptime("31 Nov2000", "%d %b 20%y")注意，年的数值没有20，如2010年，python认为是10年。

同时，要使用strptime，要确认是否存在该函数，使用以下代码

[py]
try:
 strptime = time.strptime
except AttributeError:
 from strptime import strptime
[/py]

<code><span style="color: black; font-family: Lucida Console; font-size: 8pt;">
</span></code>

将时间值转换为时间元组tm = time.localtime(t)

将时间元组转换为时间值：time.mktime(tm)

黏贴一下我的python代码，扫描一个txt文件，根据每行时间信息筛选信息，将符合要求的信息复制到另一个文件

[py title="example.py"]
import time
#import strptime
try:
strptime = time.strptime
except AttributeError:
from strptime import strptime

f=open(&quot;d:\\1totalCheckins.txt&quot;,'r')
out=file(&quot;d:\\1out.txt&quot;,&quot;w&quot;)

#第一行，把时间值存储在lastt中,初始化
lastline=f.readline()
arr=lastline.split('\t')
print arr
lastt=time.mktime(strptime(arr[1],&quot;20%y-%m-%dT%H:%M:%SZ&quot;))
lastflag=True
i=0
#后面的每行与前一行判断，如果user相同且时间在五分钟之内，则放弃，否则写入到out。txt中
#（默认为不同用户之间最早的和最晚的签到时间不可能在五分钟之内）
while True:
line=f.readline()
arr=line.split('\t')
i=i+1
 ? ? ??if i%1000==0:
 ? ? ??? ? ??print i,&quot;:&quot;,arr
 ? ? ??#获得时间值
 ? ? ??intt=time.mktime(strptime(arr[1],&quot;20%y-%m-%dT%H:%M:%SZ&quot;))

 ? ? ??if lastt-intt&gt;300:
 ? ? ??? ? ??if lastflag:
 ? ? ??? ? ??? ? ??out.write(lastline)
 ? ? ??? ? ??lastflag=True
 ? ? ??else:
 ? ? ??lastflag=False
 ? ? ??lastt=intt;
 ? ? ??lastline=line;
 ? ? ??if(&quot;xx&quot; in arr[4]):
 ? ? ??? ? ??print &quot;end:&quot;,arr[4],&quot;len:&quot;,len(arr[4] and &quot;xx&quot;)
 ? ? ??? ? ??break
out.flush();
out.close();
[/py]
