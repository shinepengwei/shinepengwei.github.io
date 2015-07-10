---
layout: post
title: 嵌套类——为何使用嵌套类
category: "编程"
tag: "C++"
---
嵌套类是在一个类A中定义的类B，类A为外围类，类B为嵌套类。

嵌套类的主要目的在于隐藏类的名字，从而减少全局的标识符，限制并告诫用户是否可以使用该类建立对象。这样提高了抽象能力，并强调了两个类之间的主从关系。

例子：

[cpp title="example.php"]

Class A{

Public:

ClassB{

Public:

…

}

Void f();

Private:

Int a;

}

[/cpp]

在类A的外部，如果要使用初始化B，则使用A::B object_b;

嵌套类仅仅是语法上的嵌入，嵌套类和外部类之间并没有访问权限。

<span style="color: #c00000;">嵌套类的用处：嵌套类就是为了强调嵌套类和外部类的关系，同时减少全局类的数量。比如有两个图类，无向图TUNGraph和有向图TNGraph，它们都有一个嵌套类TNodeI用于遍历节点，如果用全局类的话就要设计两个TNodeI类的名字，可能是TUNodeI和TNodeI，这样不美。而通过嵌套类，只要使用TUNGraph::TNodeI以及TNGraph::TNodeI即可。
</span>
