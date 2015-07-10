---
layout: post
title: 类成员变量初始化
category: "编程"
tags: "C++"
---
<h1>1 C++成员变量初始化方式</h1>
<div>
<table style="border-collapse: collapse;" border="0"><colgroup> <col style="width: 369px;" /> <col style="width: 369px;" /></colgroup>
<tbody valign="top">
<tr>
<td style="padding-left: 9px; padding-right: 9px; border: solid black 0.5pt;">一般变量 int</td>
<td style="padding-left: 9px; padding-right: 9px; border-top: solid black 0.5pt; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;">初始化列表、构造函数内</td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;">静态成员变量 static int</td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;">类外初始化</td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;">常量 const int</td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;">初始化列表</td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;">静态常量 static const int</td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;">类定义时</td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;">成员对象 Class</td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;">初始化器</td>
</tr>
<tr>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: solid black 0.5pt; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;">成员对象指针 Class *</td>
<td style="padding-left: 9px; padding-right: 9px; border-top: none; border-left: none; border-bottom: solid black 0.5pt; border-right: solid black 0.5pt;">初始化列表、构造函数内，和一般变量相同</td>
</tr>
</tbody>
</table>
</div>
Example：

[cpp]

#include &lt;iostream&gt;

#include &lt;string&gt;

using namespace std;

class Test

{

private:

int a;

static int b;

const int c;

static const int d=4;

public:

Test():c(3) //a(1)或者在初始化列表里初始化

{

a=1;

}

};

int Test::b=2;

void main()

{

Test t;

}

[/cpp]
<h1>2 C++类成员初始化顺序</h1>
<ol>
	<li>初始化列表优先于构造函数（初始化列表的初始化顺序与声明顺序相同，与初始化列表顺序无关）</li>
	<li>静态成员优先于实例变量</li>
	<li>父类成员变量优先于子类成员变量</li>
	<li>父类构造函数优先于成员对象构造函数优先于子类构造函数</li>
</ol>
Example：

[cpp]

using namespace std;
class Test
{
public:
	Test(string n)
	{
		cout&lt;&lt;n&lt;&lt;endl;
	}
};
class Base
{
public:
	static Test* a;
	Test* b;
	Test* c;
	Base():b(new Test(&quot;b&quot;))
	{
		c=new Test(&quot;c&quot;);
	}
	virtual ~Base()
	{
		if(a) delete a;//似乎是很欠妥的做法
		if(b) delete b;
		if(c) delete c;
	}
};
Test* Base::a=new Test(&quot;a&quot;);

class Derived:Base
{
public:
	static Test* da;
    Test* db;
	Test dc;
	Derived():dc(&quot;dc&quot;)
	{
		db=new Test(&quot;db&quot;);
	}
	~Derived()
	{
		if(da) delete da;//似乎是很欠妥的做法
		if(db) delete db;
	}
};
Test* Derived::da=new Test(&quot;da&quot;);

void main()
{
	Derived d;
} ///:~

[/cpp]

结果：

<img src="http://www.codingart.info/wp-content/uploads/2013/06/060113_1544_1.png" alt="" />

分析：首先根据顺序初始化static成员：a,da，初始化Derived首先初始化基类b,c，然后初始化成员对象dc（初始化器），最后初始化一般的变量db。
<h1>3 Java中成员变量初始化顺序</h1>
类成员变量优先于类的构造函数

静态成员优先于实例变量

父类成员变量优先于子类成员变量

父类构造函数优先于子类构造函数。

Java不精，就不举例了。

Reference：http://blog.csdn.net/jhj735412/article/details/7520528
