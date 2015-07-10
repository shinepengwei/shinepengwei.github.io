---
layout: post
title:  cocos中的对象管理-引用计数
category: "游戏研发"
---

cocos内存管理使用了引用计数的思想，即记录每个对象被其他对象持有（引用）的数量，当被引用次数为0即没有其他对象需要使用该对象时，该对象自动销毁。

在cocos中，使用Ref类对引用计数进行管理，而几乎所有的cocos类都继承于Ref类，如最常用的direcotr、node等。

此外，参与引用计数管理的类还包括：

AutoreleasePool：自动释放对象池，保寸调用过autorelease函数的对象

PoolManager：使用vector管理自动释放对象池。vector中至少有一个autoreleasePool实例，是engine创建的。

Ref主要的定义代码如下：

    class CC_DLL Ref
    {
    public:
    void retain();
    void release();
    Ref* autorelease();
    protected:
    Ref();
    protected:
    unsigned int _referenceCount;
    friend class AutoreleasePool;
    };



当一个对象使用new分配内存时，其引用计数为1，调用retain函数说明持有该对象，引用计数+1，release函数引用计数-1，当为0时销毁该对象。
此外，Ref类的构造函数为protected，即不能直接实例化，只能继承，不能直接创建。

如cocos中的内存对象管理过程如下：


    auto node=new Node(); //引用计数为1
    addChild(node); //引用计数为2
    ……
    node->removeFromParent(); //引用计数为1
    node->release(); //引用计数为0，对象被删除


autorelease函数将对象放入autorelesePool，引用计数不变，将在此帧所有的处理结束时统一release autorelesePool中的对象。

我们看下autolease函数和director中的mainLoop函数


    Ref* Ref::autorelease()
    {
        PoolManager::getInstance()->getCurrentPool()->addObject(this);
        return this;
    }
    void DisplayLinkDirector::mainLoop()
    {
        ...
        drawScene();
        PoolManager::getInstance()->getCurrentPool()->clear();//将pool中的所有对象release一遍
    }


在node类中，create函数的过程如下：


    auto node=new Node(); //引用计数为1
    node->autorelease(); //加入”智能指针池”


这个node对象在此帧处理的最后如果引用计数仍为1，则被销毁掉。而如果使用parentNode:addChild(node)将node作为parentNode的孩子节点，则他的引用计数变为2，在此帧处理结束的时候，引用计数变为1，即只有parentNode对node持有。

在cocos中，普遍使用create生成一个对象，原因就在于create封装了new操作和autorelease函数调用。

##autoreleasePool和PoolManager

autoreleasePool存储调用过autolease函数的对象，并在帧处理结束时集体release。

而PoolManager管理autoreleasePool队列，即可能存在多个autoreleasePool。那么，问题来了：为什么需要多个autoreleasePool。

答案：对象的生命周期管理。

应用背景：有的节点不需要生存"一帧"，可能需要使用结束后就直接删掉。而如果使用create创建node，不能手动release，因为create中已经调用了autolease函数。每个引用计数只能和一个release/autolease对应。

我们来看autoreleasePool的构造函数和析构函数：


    AutoreleasePool::AutoreleasePool()
    {
    _managedObjectArray.reserve(150);
    PoolManager::getInstance()->push(this);
    }
    AutoreleasePool::~AutoreleasePool()
    {
    clear();
    PoolManager::getInstance()->pop();
    }


AutoreleasePool在构造函数中将自身指针添加到PoolManager的AutoreleasePool队列中，并在析构函数中从队列中移除自己。

由于前面讲述的Ref::autorelease()始终将自己添加到“当前AutoreleasePool”中，只要当前AutoreleasePool始终为队列尾端的元素，声明一个AutoreleasePool对象就可以影响之后的对象，直到该AutoreleasePool对象被移除队列。

这样在程序中我们就可以这么使用：


    Class MyClass : public Ref
    {
    static MyClass* create(){
    auto ref=new MyClass();
    return ref->autorelease();
    }
    }
    
    void customAutoreleasePool()
    {
    AutoreleasePool pool;
    auto ref1=MyClass::create();
    auto ref2=MyClass::create();
    }


在该方法开始执行时，声明一个AutoreleasePool类型的自动变量pool，其构造函数会将自身加入的PoolManager的AutoreleasePool队列尾端，接下来ref1和ref2都会被加入到pool池中，当该方法结束时，pool自动变量的生命周期结束，其析构函数将会释放对象，并从队列中移除自己。

这样我们就能够通过自定义AutoreleasePool的生命周期来控制Cocos2d-x中“智能指针”的生命周期。
注意：不要动态分配AutoreleasePool对象，始终使用局部变量，这样AutoreleasePool对象会自动调用析构函数。



Reference:
http://www.cnblogs.com/mmidd/p/3746256.html

http://hielvis.com/2014/04/16/cocos2d-x-memory/

