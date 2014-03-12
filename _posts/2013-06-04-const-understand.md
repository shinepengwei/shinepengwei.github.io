---
layout: post
title: Const深入解析
category: "编程"
tags:
- C++
---
Const关键词的引入主要是为了区分什么可以修改，什么不可以修改。同时，它可以保证代码的安全性并提供访问控制保证。C中使用#define进行值替换，const可以提供这种功能，但是不仅仅限于如此简单的应用。它还可以通过对指针、函数参数、返回值、类和成员函数的修饰，对其进行控制和说明。尽可能使用const是一种良好的代码风格，因为它可以保证减少bug。

我曾经写过一篇简单介绍<a href="http://www.codingart.info/index.php/const/">const用法的文章</a>，这里将更加详细的介绍和解析。主要参考书《Thinking in C++》（英文版）。
<h1>1 const基础用法和概念</h1>
C语言中，使用#define BUFSIZE 100进行值替换，预处理器将所有的BUFSIZE简单的替换成了100，但没有类型信息往往会出现隐藏的bug。

在C++中，可使用const int bufsize=100;虽然bufsize是一个变量的形式，但是其并没有分配内存，而且编译器在编译阶段可获知bufsize的值并且进行constant folding（就是bufsize+10，编译器直接计算完成）。也可以char buf[bufsize];

Const出了修饰常量以外，它还可以修饰变量。当一个变量通过运行时数据初始化以后，你知道这个变量就不会改变。那也可以使用const对其修饰，以后也就不能改变它的值。这是一个好的变成习惯，而不是要求。

<strong>编译器处理const的方式
</strong>

<span style="font-size: 9pt;">关键词：内链接，不分配内存
</span>

Const变量默认是内链接的，也就是说只能被本编译单元使用。因此，const变量一般定义在.h文件中，需要使用它的编译单元通过#include使用。

对于const常量，编译器一般不为其分配空间，只在符号表(symbol table)中记录常量的定义。即编译器可以知道常量的值，因而能通过它定义数组长度。但是，当const变量显式定义外链接extern时，或者对其使用取地址操作，编译器就会为其分配一个内存空间。除此之外，当const修饰复杂的数据结构时，如数组，编译器不会将其放入符号表，也为其分配内存空间。

不放入符号表也就代表着编译阶段不能使用它的值，如

const int i[] = { 1, 2, 3, 4 };

//! float f[i[3]]; // Illegal

将const变量显式定义为外链接的方法如下：

extern const int x = 1;

在需要使用该const的地方进行声明：

extern const int x;

上面通过初始化区分定义和声明。
<div>
<table style="border-collapse: collapse;" border="0"><colgroup> <col style="width: 738px;" /></colgroup>
<tbody valign="top">
<tr>
<td style="padding-left: 9px; padding-right: 9px; border: solid black 0.5pt;"><span style="font-size: 10pt;">Const常量为什么要使用初始化区分定义和声明？
</span>

<span style="font-size: 10pt;">答：在传统的全局变量中，int x;既代表声明，有代表定义，此处分配内存，默认为外连接。在需要使用x的编译单元，通过extern const int x；作为定义，表示在其他编译单元有x的定义，即此处不会为其分配内存。而对于const常量，要使用extern显式定义以此表示外联结，无法通过使用传统的方法区别定义和声明，因此使用初始化区分。</span></td>
</tr>
</tbody>
</table>
</div>
<h1>2 const修饰指针</h1>
Const修饰指针，可以表示指针指向的地址存储的是常量，也可以表示该指针不能被修改。

指针指向的是const，有两种表示方法：(1) const int* u;(2) int const* v;

Const指针的表示方法为：

int d = 1;

int* const w = &amp;d;

指向const的指针可以指向非const对象，只要保证不通过指针改变该对象。但是，不能将const对象的地址复制给指向非const的指针，因为可能通过这个指针对const对象进行修改。
<h1>3修饰函数参数和返回值</h1>
用const可以修饰函数参数和返回值，如果按值传递的话，其实const没有啥意义，因为它仅表示传的参数在函数内不能被修改，但是参数即使修改了对函数外也没有任何影响，所以没有意义。但是如果通过按地址传递，const就有很大的作用。

<span style="font-size: 8pt;">注：在书中有一小节对C++中返回值为对象置为const进行了介绍，但是我觉得有歧义，可能是C++中原本就存在的一些限制和兼容性想法。总之，我没太看懂，就不写了。
</span>

这里主要介绍const修饰按地址传递（指针、引用）的函数参数和返回值，也是平时最经常应用的情况。

在按地址传递的函数中，应该尽可能将其声明为const。

在C++中，参数传递的第一选择是使用const引用，尤其是参数为自定义类型。使用const引用有很多好处，比如不用复制对象，使用者的使用格式和按值传递相同。使用const引用还有一个好处就是可以将临时对象作为函数参数，因为临时对象是const类型的。
<h1>4 const和Class</h1>
当用const修饰和类有关的元素时，他也提供了一些功能。如const成员变量，static const成员变量，const成员函数以及const对象等。

4.1 const成员变量

这里翻译为<strong>const成员变量</strong>而不是<strong>const成员常量</strong>，原因是const修饰的成员变量并不是固定的，对于不同的对象可有不同的值，const修饰表示该变量在初始化以后在对象的生命周期内不能被修改。

当const修饰成员变量时，const的使用机制和C中的const相似（C中很少使用const，但是确实存在这个关键词），会为const变量分配内存，确切的说，是为每个对象都分配一个内存存放const成员变量。

const成员变量的初始化和其他成员变量不同，因为在构造函数体中，const应该已经被初始化而且就不能再被修改。因此，const成员变量使用初始化列表初始化，表示在构造函数被执行之前就进行了初始化。

4.2 编译时const常量

Const成员变量并不能作为一个常量让编译器直接使用，因为它仅表示对象生命周期内不能修改，但是每个对象都可以有不同的值。如果想实现编译时常量，需要使用static关键词。

Static const常量必须在声明时初始化，注意，也只有static const常量可以这样初始化，其他变量都不可以。

如：

class StringStack {

static const int size = 100;

const string* stack[size];

};

4.3 const对象和const成员函数

对于const对象，编译器保证在对象的生命周期没有成员变量被修改。编译器可以保证没有public的成员变量被修改，但是如何保证调用的成员函数没有修改成员变量呢。这里就需要用到const成员函数，当用const修饰成员函数时，我们告诉编译器该函数可被const对象调用，而且该函数不会修改对象的成员变量。没有声明为const的成员函数默认为会修改对象，不能被const对象调用。（即使函数没有修改对象，只要没有声明为const，也不能被const对象调用，因此如果函数没有修改成员变量就应该将其声明为const）。

任何被声明为const的成员函数都不能修改对象，编译器要求它不能修改成员变量，也不能调用非const的成员函数。

Const修饰成员函数必须在声明和定义中都显式修饰：

class X {

int i;

public:

X(int ii);

int f() const;

};

X::X(int ii) : i(ii) {}

int X::f() const { return i; }

const成员函数能被const或者非const对象访问，从这个角度，我们也应该尽量将成员函数声明为const。
