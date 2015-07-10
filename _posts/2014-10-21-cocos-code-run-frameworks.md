---
layout: post
title:  cocos入口以及整体框架
category: "游戏研发"
---

本文介绍cocos的入口代码以及整体框架，此外，本文代码只显示重要的部分代码，详细代码请看源代码。

首先介绍cocos的入口，以cpp-empty-test工程为例。

main.cpp中程序的入口为：

```

	AppDelegate app;
	return Application::getInstance()->run();

```

在Application类中，存在一个run函数，这个函数使整个cocos程序的运行入口。

这个函数主要的功能为：

1.配置windows窗口

2.初始化Director导演对象（cocos程序中只有一个）

3.循环调用director对象的mainLoop函数。

其中第三点为cocos程序运行的核心，程序运行过程即为一直循环的过程。代码如下：

```

	 while(!glview->windowShouldClose())
	{
	    if (nNow.QuadPart - nLast.QuadPart > _animationInterval.QuadPart)
	    {
	        nLast.QuadPart = nNow.QuadPart;
	        director->mainLoop();
	        glview->pollEvents();
	    }
	    else
	    {
	        Sleep(0);
	    }
	}

```

判断是否关闭窗口，如果不关闭首先计算两次运行的时间间隔是否符合已经设置的帧间隔。如默认为每秒60帧，则两次运行时间不到1/60s的话就sleep，否则进入director的mainLoop函数。

Director类的mainLoop函数每一帧调用一次，主要功能为1：调用drawScene函数。2.内存管理

```C

	void DisplayLinkDirector::mainLoop()
	{
	    if (_purgeDirectorInNextLoop) --end()调用后会设为true
	    {
	        ...//purge director
	    }
	    else if (! _invalid)
	    {
	        drawScene();--绘制
	        // release the objects 内存管理
	        PoolManager::getInstance()->getCurrentPool()->clear();
	    }
	}
```

其中最重要的为drawScene函数。drawScene函数的功能包括三部分：1场景绘制，2scheduler的update，3事件分发

```

	void Director::drawScene()
	{
	    ...
	    //tick before glClear: issue #533
	    if (! _paused) --调度器update
	    {
	        _scheduler->update(_deltaTime);
	        _eventDispatcher->dispatchEvent(_eventAfterUpdate);
	    }
	    // to avoid flickr, nextScene MUST be here: after tick and before draw.
	    if (_nextScene)
	    {
	        setNextScene(); --进入下一个场景，再上一帧中设置_nextScene，这一帧进行替换
	    }
	    // draw the scene
	    if (_runningScene)
	    {
	        _runningScene->visit(_renderer, Mat4::IDENTITY, false);
	        _eventDispatcher->dispatchEvent(_eventAfterVisit);
	    }
	    _renderer->render();
	    _eventDispatcher->dispatchEvent(_eventAfterDraw);
	
	}
```

总的来说，我认为cocos代码中比较重要的分为4块：

- 场景绘制
- 内存管理
- 调度器update
- 事件分发处理

后面的文章我将对每块分别进行总结和解析。
