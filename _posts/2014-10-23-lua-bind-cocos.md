---
layout: post
title:  LUA和cocos中函数的绑定以及lua和C++的相互调用
category: "游戏研发"
---

在lua_cocos2dx_auto.cpp文件中，有大量的供lua调用C++函数的中介函数，如下面的函数是Node:getPositionX函数的中介函数，lua里面的getPositionX函数本质上调用了下面的函数，然后下面的函数又调用了cocos C++中的getPositionX函数。

    int lua_cocos2dx_Node_getPositionX(lua_State* tolua_S)
    {
        int argc = 0;
        cocos2d::Node* cobj = nullptr;
        bool ok  = true;
        cobj = (cocos2d::Node*)tolua_tousertype(tolua_S,1,0);
        argc = lua_gettop(tolua_S)-1;
        if (argc == 0) 
        {
            if(!ok)
                return 0;
            double ret = cobj->getPositionX();
            tolua_pushnumber(tolua_S,(lua_Number)ret);
            return 1;
        }
        return 0;
    }
除了中介函数，还要将中介函数绑定供lua使用，在lua_cocos2dx_auto.cpp每个cocos类都有一个以下格式的函数：

    int lua_register_cocos2dx_Node(lua_State* tolua_S)
    {
        tolua_usertype(tolua_S,"cc.Node");
        tolua_cclass(tolua_S,"Node","cc.Node","cc.Ref",nullptr);
        tolua_beginmodule(tolua_S,"Node");
            tolua_function(tolua_S,"addChild",lua_cocos2dx_Node_addChild);
            tolua_function(tolua_S,"getPositionX",lua_cocos2dx_Node_getPositionX);
        tolua_endmodule(tolua_S);
        std::string typeName = typeid(cocos2d::Node).name();
        g_luaType[typeName] = "cc.Node";
        g_typeCast["Node"] = "cc.Node";
        return 1;
    }
    


这样，lua中就可以直接通过调用getPositionX函数，调用lua_cocos2dx_Node_getPositionX中介函数，从而进一步调用了c++中的getPositionX函数。

----

##Lua和C++交互原理

在cocos中lua绑定就是这些函数，这里稍微说下lua和C++的交互原理。

参考：http://blog.csdn.net/musicvs/article/details/8440919 （这哥们的文章可用于入门，但是比较简单，此外，我看的要精神分裂了，尼玛）

lua和C++的交互必须通过堆栈，交互流程如下图所示：

![Alt text](http://www.codingart.info/images/201410/1356522919_2602.jpg)



    -- hello.lua 文件
    myName = "beauty girl"
    
一个lua文件如上，如果C++要访问lua中myName变量，则需要以下过程：

1） C++想获取Lua的myName字符串的值，所以它把myName放到Lua堆栈（栈顶），以便Lua能看到

2） Lua从堆栈（栈顶）中获取myName，此时栈顶再次变为空

3） Lua拿着这个myName去Lua全局表查找myName对应的字符串

4） 全局表返回一个字符串”beauty girl”

5） Lua把取得的“beauty girl”字符串放到堆栈（栈顶）

6） C++可以从Lua堆栈中取得“beauty girl”

C++中访问lua变量myName的代码如下：

    bool HelloLua::init() {
        lua_State* pL = lua_open();
        luaopen_base(pL);
        luaopen_math(pL);
        luaopen_string(pL);
        
        /* 1.执行Lua脚本,返回0代表成功 */
        int err = luaL_dofile(pL, "helloLua.lua");
        CCLOG("open : %d", err);
        
        /* 2.重置栈顶索引 */
        lua_settop(pL, 0);
        lua_getglobal(pL, "myName");
        
        /* 3.判断栈顶的值的类型是否为String, 返回非0值代表成功 */
        int isstr = lua_isstring(pL, 1);  
        CCLOG("isstr = %d", isstr); 
        
    　　/* 4.获取栈顶的值 */
    　　const char* str = lua_tostring(pL, 1);  
        CCLOG("getStr = %s", str);  
        
        lua_close(pL);
        return true;
    }

解释一些内容：

1. C++要使用Lua，必须要有一个lua_State，所有的Lua脚本文件都在一个lua_State中运行。
2. luaopen_XXX表示需要去加载lua重的一些标准库
3. lua_settop(pL, 0);为了让栈顶索引置为0，这样操作栈就可以根据索引来操作。（再入栈的元素索引即为1）
4. lua_getglobal(pL, "myName");就是C++将字符串myName放在栈顶，然后lua取出字符串并根据字符串取出全局变量的值放在栈顶，即执行刚才我们所说的6步中的前5步。
5. const char* str = lua_tostring(pL, 1);从堆栈获得lua放进去的字符串。
6. lua_close(pL);释放pL内存

lua_toNumber,lua_tofunction等和lua_tostring一样，都是从堆栈获得相应类型的值。

此外，C++中使用lua的函数，需要include下面的c库。

    extern "C" {  
    #include <lua.h>  
    #include <lualib.h>  
    #include <lauxlib.h>  
    };  
    
C++调用lua的函数不说了，用的比较少，可以参考;
http://blog.csdn.net/musicvs/article/details/8451361

----
##Lua调用C++函数
上面讲的都是C++调用lua中的元素，下面说一下lua如何调用C++中的函数，lua和C++的交互在cocos也是这种应用。

首先有一个C++函数如下：

    public:
    　　static int getNumber(int num);

    int HelloLua::getNumber( int num ) {
        CCLOG("getNumber num = %d", num);
        return num + 1;
    }
    
这个函数lua不能直接调用，需要一个中介函数：

    public:
    static int cpp_GetNumber(lua_State* pL);
    
    
    int HelloLua::cpp_GetNumber( lua_State* pL ) {
        /* 从栈顶中取一个值 */
        int num = (int)lua_tonumber(pL, -1);
    
        /* 调用getNumber函数，将返回值入栈 */
        lua_pushnumber(pL, getNumber(num));
    
        /* 返回值个数，getNumber只有一个返回值，所以返回1 */
        return 1;
    }
    
稍微解释下这个函数：
1. 从Lua和C++交互的栈先获得一个值作为函数参数
2. 然后再将函数返回值放入栈，
3. 最后返回返回值的个数。


那么lua文件如何调用中介函数呢，如下;

    --C++中首先注册函数
    bool HelloLua::init() {  
        lua_State* pL = lua_open();  
        luaopen_base(pL);  
      
        /* C++的函数和封装函数都必须是静态的，不知道可不可以不是静态的？当然不可以 */  
        lua_register(pL, "cpp_GetNumber", cpp_GetNumber);  
        luaL_dofile(pL, "helloLua.lua");  
        lua_close(pL);  
        return true;  
    }
    
    --Lua可以直接调用函数
    local num = cpp_GetNumber(10);

解释一下：
1.lua_register将C++静态函数注册到Lua中
2.必须是静态函数。

为什么？

很简单，如果不是静态函数，就必须在对象被创建之后才能调用。在Lua中是不能也不会去再次创建一个HelloLua对象的（当然，在Lua中还是能够创建C++对象的，这个暂时不讨论），因此，注册的函数必须是静态的。

附：

详细问题看系列教程：

http://blog.csdn.net/musicvs/article/details/8451369

----

##cocos中实现绑定的方法

以上就是lua和C++的相互调用原理方法，但是！cocos中lua并不是这样调用C++函数的（C++中介函数还是有的），或者说，**至少不需要我们去写这些中介函数和绑定代码**。

cocos中如何实现lua调用C++函数的呢？

cocos3.0以后使用bindings-generator工具自动生成中介函数和lua绑定注册代码。

简单的来说，bindings-generator工具基于ToLua++根据配置自动生成中介函数、注册函数代码（全部为C++），这样Lua就直接可以调用函数了。

关于bindings-generator参考：
http://www.cocos2d-x.org/docs/manual/framework/native/wiki/how-to-use-bindings-generator/zh
https://github.com/cocos2d/bindings-generator   

toLua++：
http://www.codenix.com/~tolua/tolua++.html
