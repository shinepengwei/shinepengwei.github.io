---
layout: post
title:  Effective C++ 读书笔记
category: "编程"
tags: 
- "C++"
- "读书笔记"

---


条款1：视C++为一个联邦语言
<div>C++主要的次语言：C，Object-oriented C++，Template C++，STL。</div>
<div>

<hr />

</div>
<div>条款2：尽量以const，enum，inline代替#define</div>
<div>尽量以编译器替换预处理器，使用#define是预处理器的工作，他并没有进入记号表。</div>
<div>使用const替换#define，有两个特殊情况是#define无法实现的。1.定义常量指针。2.class的专属常量。对于class的专属常量，为了确保所有对象只有一份，会将它声明为static成员。在类中声明如：class Tmp{private:static const int NUM=5;...};这是一个声明式，通常C++都要求对所有元素都提供定义式，但是对于static const成员来说，如果不取他的地址，就不需要定义式。定义式： const int Tmp::NUM;注意，#define无法创建class专属常量，因为他不重视作用域。</div>
<div>#define还有一个使用方法就是通过定义宏来将其类似函数的使用，而不招致函数调用开销。但是宏使用起来容易出错，可以使用Template inline函数代替之。</div>
<div>

<hr />

条款3：尽可能使用const

</div>
<div>const允许指定语义约束：不该被改动。</div>
<div>对于指针，如果const出现在星号左边表示被指物是常量，星号右边表示指针本身是常量。</div>
<div>对于迭代器，如果希望迭代器指向的东西不可被改变，需要使用const_iterator，如果迭代器不能改变，使用const iterator.</div>
<div>对于一个函数，const可以修饰函数返回值、参数、函数本身</div>
<div>const成员函数表示该成员函数对对象不改变，可作用于const对象。注意，两个同名成员函数一个const，一个非const也可以被重载。</div>
<div>const有两个流派：bitwist constness和logical constness.</div>
<div>非const成员函数调用const没有问题，但是const成员函数不能调用非const成员函数。</div>
<div>

<hr />

条款4：确定对象被使用前已先被初始化

</div>
<div>对于内置类型，手工完成初始化。对于内置类型以外的其他东西，初始化责任在于构造函数，确保每个构造函数都对每个成员初始化。</div>
<div>要明白赋值和初始化之间的区别。尽量以初始化列表初始化，和在构造函数中使用赋值语句相比，他效率更高。因为初始化列表是在初始化时直接使用值初始化，而后者是先使用默认构造函数初始化成员，然后进行赋值。</div>
<div>对于const和reference，他一定需要初值，而不是赋值。</div>
<div>初始化次序是固定的：基类早于派生类，一个类的成员变量根据其声明次序被初始化。</div>
<div align="left"><span style="font-family: 宋体;">对于不同编译单元内的</span> <span style="font-family: Tahoma;">non-local static</span><span style="font-family: 宋体;">对象，</span> <span style="font-family: Tahoma;">C++</span><span style="font-family: 宋体;">并没有定义他们的初始化次序，那如果一个编译单元内的</span> <span style="font-family: Tahoma;">non-local static</span><span style="font-family: 宋体;">对象需要访问另一个编译单元内的</span> <span style="font-family: Tahoma;">non-local static</span><span style="font-family: 宋体;">对象，我们如何保证它已经初始化过了。方法是：将每个</span> <span style="font-family: Tahoma;">non-local static</span><span style="font-family: 宋体;">对象都放入一个函数中，使其转换为</span> <span style="font-family: Tahoma;">local static</span><span style="font-family: 宋体;">，在使用这个对象调用这个函数返回这个对象，这样就可以保证这个</span> <span style="font-family: Tahoma;">static</span><span style="font-family: 宋体;">初始化过了。</span></div>
<div>

<hr />

</div>
<div>条款5 了解C++默默编写并调用了哪些函数</div>
<div>编译器会自动声明默认构造函数、拷贝构造函数、拷贝运算符函数和析构函数。这些函数都是public且inline的。</div>
<div>这些函数被调用，才会被编译器创建。</div>
<div>拒绝生成拷贝赋值运算符函数：1.有reference，2.有const成员，3.某基类将copy赋值运算符声明为private。</div>
<div>

<hr />

条款6 若不想使用编译器自动生成函数，就该明确拒绝

</div>
<div>如果不想要拷给构造函数和赋值运算符函数，就应该将它们声明为private，不需要定义它。</div>
<div>或者定义一个Uncopyable类，根据上一个方法实现，然后禁止被拷贝的函数都继承他。</div>
<div>

<hr />

条款7 为多态基类声明virtual析构函数

</div>
<div>当class内含有至少一个virtual函数，才为它声明virtual析构函数。</div>
<div>如果一个类的设计目的不是作为基类使用，或者不是为了具备多态性，就不应该声明virtual函数。</div>
<div>

<hr />

条款8 别让异常逃离析构函数

</div>
<div>析构函数绝对不要吐出异常</div>
<div>

<hr />

条款9 绝不在构造和析构过程中调用virtual函数

</div>
<div>在基类构造过程中，virtual不是虚函数。</div>
<div>在派生类对象的基类构造期间，对象的类型是基类而不是派生类。不只是virtual函数会被编译器解析到基类，此时若是使用运行期类型信息（如dynamic_cast和typeid），也会把对象看作为基类类型。同样的道理也适合于析构函数。</div>
<div>

<hr />

条款10 令operator=返回一个reference to *this.

</div>
<div>赋值一般可以写为连锁形式：x=y=z=15，同时，赋值遵循右结合律，即上述连锁赋值等同于：x=(y=(z=15))</div>
<div>为实现这种连锁赋值，赋值运算符必须返回一个referrence只想运算符的左侧，即赋值运算符需要遵循一下操作：</div>
<div>widget &amp; operaotr=(const Widget &amp; rhs){</div>
<div>     ...</div>
<div>     return * this;</div>
<div>}</div>
<div>

<hr />

</div>
<div>条款11 在operator=中处理自我赋值</div>
<div>需要注意，在operator=函数中，开始时需要考虑如果是自我赋值怎么办。</div>
<div>

<hr />

条款12 复制对象时不要忘了每一个成分

</div>
<div>拷贝函数包括拷贝构造函数和拷贝符操作符函数。</div>
<div>拷贝每一个成分包括：1.复制所有的local成员变量。2.调用所有的基类中适当的拷贝函数。</div>
<div>不要尝试以某一个拷贝函数通过调用另一个拷贝函数实现。需要的话，应该将其放入一个共同的函数，然后两个都去调用这个函数。</div>
<div>

<hr />

条款13 以对象管理资源

</div>
<div>使用new创建一个对象后，要记得使用delete释放这个对象。为了确保这个对象总是被释放，不会因为使用者遗忘释放而产生内存泄漏，可以将资源（这个对象指针）放进另外一个对象（管理对象）内，当管理对象自动调用析构函数时，资源自动释放。</div>
<div>两个关键想法：1.获得资源后立即放入管理对象。2.管理对象运用析构函数确保资源被释放。</div>
<div>标准程序库提供了auto_ptr可以作为资源的管理者，在资源获取到的时候立即初始化auto_ptr。</div>
<div>在使用suto_ptr管理对象的时候，注意不能让多个auto_ptr指向同一对象，如果这样的话对象会被删除一次以上。因此，对于auto_ptr来说，拷贝操作会将被拷贝者变为NULL。</div>
<div>为了解决以上问题，可以使用“引用计数型智慧指针RCSP”代替auto_ptr。RCSP也是一个智能指针，它持续追踪有多少个对象指向这笔资源，当无人指向它的时候自动删除。</div>
<div>std::auto_ptr;</div>
<div>std::tr1::share_ptr;</div>
<div>对于资源管理对象来说，析构函数自动调用的是delete而不是delete[]，所以管理对象不能基于数组使用。</div>
<div>

<hr />

</div>
<div>条款14 在资源管理类中小心拷贝行为</div>
<div>在资源管理类中的拷贝行为可以有两种方法，1.禁止复制。2.对底层资源使用“引用计数法”。</div>
<div>

<hr />

条款15 在资源管理类中提供对原始资源的访问

</div>
<div>tr1::share_ptr和auto_ptr提供了一个get成员函数，他会返回一个指针内部的原始指针。同时，他们重载了指针取值操作符（-&gt; and *）</div>
<div>对原始资源的访问包括：显式转换和隐式转换。显式转换比较安全，隐式转换比较方便。</div>
<div>隐式转换：</div>
<div>class A{</div>
<div>public:</div>
<div>     operaotr B() const{//隐式转换函数</div>
<div>          return b;</div>
<div>     }</div>
<div>}</div>
<div>

<hr />

条款16 成对使用new和delete时采取相同形式

</div>
<div>在new表达式中使用[]，必须在相应的delete表达式中使用[]。</div>
<div>

<hr />

</div>
<div>条款17 以独立语句将new完成的对象放入智能指针</div>
<div>如果不这样，异常可能导致资源泄漏。</div>
<div>

<hr />

<div>设计与声明</div>
</div>
<div>条款18 让接口容易被使用，不易被误用</div>
<div>条款19 设计class由于设计type</div>
<div></div>
<div>条款20 宁以“传递const引用”替换“传值”</div>
<div>C++函数调用参数默认的是按值传递，传值期间需要调用拷贝构造函数</div>
<div>使用按引用传递可以避免对象切割问题：当一个派生类按值传递的时候被视为基类，拷贝构造函数只拷贝基类的对象。</div>
<div>一般来说，对于内置类型、STL迭代器和函数对象，使用按值传递比较合适。</div>
<div>

<hr />

条款21 必须返回对象时，必须返回其reference

</div>
<div>一个函数返回一个引用，若引用对象在函数中生成，那无论是堆上创建还是栈上创建，都不何时返回引用。（堆：内存泄漏；栈：错误）</div>
<div></div>
<div>若是一个函数的输入对象，就可以将其按引用输出了。如剑指offer题目：</div>
<div>面试题1 赋值运算符函数
<div>1.返回值类型声明为该类型的引用，然后函数结束前return *this.</div>
<div>2.把传入的参数的类型声明为常量引用。</div>
<div>3.首先释放实例自身已占用的内存。</div>
<div>4.判断输入的参数和当前的实例是否是一个实例</div>
</div>
<div>

<hr />

条款22 将成员变量声明为private

</div>
<div>众所周知，为了实现较好的封装性，一般将成员变量声明为private。</div>
<div>将成员变量声明为private，可赋予客户访问数据的一致性、可细微划分访问控制、允许约束条件获得保证，并提供class作者以充分的弹性。</div>
<div>

<hr />

条款23 宁以non-nember、non-friend替换member函数

</div>
<div>封装的好处是它使我们能够改变事物而只影响有限客户；而封装性的评价：越多函数可以访问它，数据的封装行就越低。</div>
<div>member函数带来的封装性比non-member函数低，因为member函数可以访问对象的private成员变量，而造成了该类较差的封装性。</div>
<div>

<hr />

条款24 若所有的参数都需要类型转换，使用非成员函数

</div>
<div>只有当参数列于参数列中，这个参数才是隐式类型转换的合理参与者。</div>
<div>也就是说，对于作为成员函数的*运算符重载，后一个操作数可以隐式转换（转换成函数参数），前一个操作数无法转换成对象。因此，使用非成员函数，这样两个操作数都可以转化为参数相应的对象。</div>
<div>

<hr />

条款25 考虑写出一个不抛异常的swap函数（<strong>没看懂，待进一步分析</strong>）

</div>
<div></div>
<div>

<hr />

</div>
<div>实现</div>
<div></div>
<div>条款26 尽可能延后变量定义式的出现时间</div>
<div>延后变量的定义，直至非得使用该变量的前一刻为止，甚至应该尝试延后这份定义直到能够给他初值实参。</div>
<div>对于循环来说，将循环体内使用到的一个对象定义在循环体外还是体内，体外的话成本是1次构造，一次析构，n次赋值；体内的话n此构造，n次析构。一般来说，体外的话更加高效（加入赋值成本低于构造+析构），但是缺点就是该对象的作用域更大，降低了可理解性和易维护性。

<hr />

条款27 尽量少做转型动作
const_cast&lt;T&gt;(expression)

</div>
<div>常量性移除</div>
<div>dynamic_cast&lt;T&gt;</div>
<div>继承体系内安全向下转型，出现错误即为NULL</div>
<div>reinterpret&lt;T&gt; 强制转换，很危险，一般不使用</div>
<div>static_cast&lt;T&gt;</div>
<div>强迫隐式转换。可以将non-const变量转为const变量。</div>
<div></div>
<div>转型动作并不是仅仅把某种类型视为另一种类型，比如int型转为float型，再比如派生类对象指针转为基类对象指针。</div>
<div>同一对象，基类指针和派生类指针指向它，地址的值是不同的</div>
<div></div>
<div>dynamic_cast成本很高，如何避免实现？</div>
<div>1.直接存储派生类指针（？废话！）</div>
<div>2.把要调用派生类中的成员函数定义在基类，啥都不做。</div>
<div>对于连串的dynamic_cast，即判断他是哪一个派生类然后做某一个动作，这种行为必须避免。可以virtual函数调用避免。</div>
<div></div>
<div>如果必须转型，可以考虑将它用函数封装，客户调用该函数而避免转型。</div>
<div>尽量使用新式转型。</div>
<div>

<hr />

条款28 避免返回handles指向对象内部成员

</div>
<div>handles指指针、引用或者迭代器等间接访问内存的方式。</div>
<div>返回指向对象内部成员的handle，破坏了其封装性。</div>
<div>破坏了成员函数的const性，因为可以对其进行修改。</div>
<div>handle存在的时间可能比所指对象存在的时间更长，导致指针空悬。</div>
<div></div>
<div>

<hr />

</div>
<div>条款30 透彻了解inlining的里里外外</div>
<div>声明方式：隐喻——函数定义于class定义式内。显式——在定义式前加上关键字inline。</div>
<div>inline函数通常被置于头文件，因为大部分编译环境在编译过程进行inline，同时inline函数一般将增加目标码体积。</div>
<div>line只是一个申请，编译器对于取函数地址的操作，或者对于太过复杂（如带有循环和递归）的函数拒绝inline。</div>
<div>一般来说，不会为构造函数和析构函数使用inline，因为编译器可能为构造函数和析构函数生成代码（如初始化基类和成员），同时也可能自动调用类的构造函数和析构函数。</div>
<div>缺点：</div>
<div>使用inline的客户升级复杂，inline函数修改了就需要重新编译所有的使用该函数的客户程序。</div>
<div>无法调试。</div>
<div></div>