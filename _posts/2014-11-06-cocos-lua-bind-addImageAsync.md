---
layout: post
title:  在cocos中为addImageAsync函数添加Lua脚本绑定
category: "游戏研发"
tag:  "Cocos"
---

==
在cocos-2dx 3.x版本中，提供了一个异步加载函数TextureCache:addImageAsync()，将资源异步加载到chache中而不占用主线程。这个函数在cocos3.1版本中并没有绑定到Lua脚本，即Lua脚本不能调用这个函数，否则提示为nil。而在3.2版本中就将其加到了Lua绑定，Lua脚本中可以使用了。

在我的博客文章《[LUA和cocos中函数的绑定以及lua和C++的相互调用](http://www.codingart.info/lua-bind-cocos.html)》中，介绍了cocos中是如何进行Lua绑定，从而使Lua脚本可以调用Cocos中的C++程序的。

总的来说，就是使用[bindings-generator](https://github.com/cocos2d/bindings-generator)根据配置自动生成C++与Lua互相调用的代码。

但是，bindings-generator到目前为止 3.0 正式版，完全没有解决 c++11 functional 与 lambda function 参数自动绑定到 lua 中的任何解决方法，而addImageAsync函数恰恰是基于C++11中多线程实现的，其中还使用了一些C++11的lambda function等特性。因此，不能自动绑定。

从cocos2d-x\tools\tolua\cocos2dx.ini配置文件中，也发现以下代码：
    skip = ...  TextureCache::[addPVRTCImage addImageAsync函数。],..
即自动生成绑定代码会忽略addImageAsync函数。

---

那么，Cocos3.2是怎么添加的呢。

在liblua项目中的manual/lua_cocos2dx_manual.cpp文件中，发现有个函数叫：`static int lua_cocos2dx_TextureCache_addImageAsync(lua_State* tolua_S)`。

这个函数就是调用addImageAsync的C++中介函数。

再朝下看发现代码：

```
    static void extendTextureCache(lua_State* tolua_S)
    {
        lua_pushstring(tolua_S, "cc.TextureCache");
        lua_rawget(tolua_S, LUA_REGISTRYINDEX);
        if (lua_istable(tolua_S,-1))
        {
            tolua_function(tolua_S, "addImageAsync", lua_cocos2dx_TextureCache_addImageAsync);
        }
        lua_pop(tolua_S, 1);
    }
```

再朝下看`register_all_cocos2dx_manual`函数中调用了extendTextureCache()函数，将addImageAsync函数绑定到Lua脚本。

在cocos3.1中，也有`register_all_cocos2dx_manual()`函数，那么我就把

```
    static int lua_cocos2dx_TextureCache_addImageAsync(lua_State* tolua_S)
    static void extendTextureCache(lua_State* tolua_S)
```

两个函数复制到了3.1，并将extendTextureCache函数在register_all_cocos2dx_manual中调用。

搞定！！！





