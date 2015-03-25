---
layout: post
title:  Lua预编译和加密
category: "游戏研发"
tag:  "Cocos"
---

可以使用cocos luacompile预编译lua代码，使用方法：http://www.cocos2d-x.org/wiki/Cocos_luacompile
在windows中测试可用，使用步骤：
1.预编译src文件夹，输出为srccompile文件夹
2.将shader等文件拷贝到srccompile文件夹中
3.把main.lua考进来，lua入口不变。
4.修改main.lua中的sys.require_path

64位IOS不可用，目前没有完整的解决方案。

预研lua预编译及加密结论：
1.64位IOS 不支持luajit，cocos自带的lua预编译及加密解决方案luacompile无法使用。（据说cocos官方会在近期就会给出解决方案）
2.可以只使用luacompile中的xxtea加密，不使用预编译的字节码。但是目前公司公用引擎3.0不支持，3.2开始支持
3.公司内部可能使用luac而不是使用luajit，那么存在两个问题：
	a.IOS64位不能使用luajit，也就是说目前不能使用cocos自带的luacompile。看到景奇说IOS使用luac，但是cocos引擎貌似是支持luajit而不支持luac的，我试了下加载luac字节码提示错误cannot load incompatible bytecode
	b.如果用luac预编译，那还需要对luac进行加密防止反编译破解。

公司或者官方应该会在游戏上线之前做出成熟的解决方案。

可用的备用方案：可以使用的方法是使用luacompile的加密功能，而不是用其预编译功能。这样可以实现加密lua文件功能。

ref:http://www.cocos2d-x.org/news/386
