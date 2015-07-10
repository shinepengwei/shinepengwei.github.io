---
layout: post
title:  Lua 和C++的相互调用
category: "游戏研发"
tag:  "Lua"
---

之前介绍过Lua和C++的相互调用原理以及cocos中如何绑定Lua和C++函数：[LUA和cocos中函数的绑定以及lua和C++的相互调用](http://www.codingart.info/lua-bind-cocos.html)

还举例介绍了在cocos中增加一个Lua绑定函数：[在cocos中为addImageAsync函数添加Lua脚本绑定](http://www.codingart.info/cocos-lua-bind-addImageAsync.html)

这篇文章讲一下Lua和C++互相调用的基本步骤：

1.首先在vs中配置C++调用lua所需要的库：
vc++目录：

![](/images/2014/2014-11-11-lua-cpp-hellowold-1.png)

![](/images/2014/2014-11-11-lua-cpp-hellowold-2.png)

链接器：

![](/images/2014/2014-11-11-lua-cpp-hellowold-3.png)


C++代码：

```

	int _tmain(int argc, _TCHAR* argv[])
	{
		
		printf("hello world\n");
		//初始化一个lua_State，之前介绍过，它是所有的Lua代码的执行环境
		lua_State* pL = luaL_newstate();
		//注册average函数
		lua_register(pL, "average", average);
		//lua中打开base/math/string库
	    luaopen_base(pL);
	    luaopen_math(pL);
	    luaopen_string(pL);
		//执行test.lua文件，err为0表示执行正确
	    int err = luaL_dofile(pL, "test.lua");
		cout<<"ERROR:"<<err<<endl;
		//取出myName变量
	    lua_settop(pL, 0);
	    lua_getglobal(pL, "myName");
		//判断栈中存储的是否是string类型，并输出
		int isstr = lua_isstring(pL, 1); 
		printf("isstr = %d", isstr);  
		string myname = lua_tostring(pL, 1);
		cout<<"\n"<<myname<<endl;
	
	    lua_close(pL);
		return 0;
	}

```

Lua代码：

```

	myName = 'hello'
	print(myName,'lua')
	local avg, sum = average(10, 20, 30, 40, 50)
	print("The average is ", avg)
	print("The sum is ", sum)

```

结果如下：

![](/images/2014/2014-11-11-lua-cpp-hellowold-4.png)