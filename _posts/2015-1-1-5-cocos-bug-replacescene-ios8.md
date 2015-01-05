---
layout: post
title:  IOS8中场景切换的BUG
category: "游戏研发"
tag:  "Cocos"
---

###描述：
在IOS8平台上cocos场景切换崩溃。

###表象：
尝试渲染无效节点。

###原因：
a.使用从场景A使用TransitionFade切换到场景B,场景B迅速切换到场景C，这时场景A到B的TransitionFade场景切换动画还没有执行完，因此崩溃。

b.场景A切换到场景B，但是场景A中还有action或者某些node的update函数还在执行，崩溃。

###解释：
a.在我们项目中使用在加载界面，IOS8加载速度快，所以从场景B迅速切换到C，出现BUG。表面上来看是IOS8上的BUG，其实可能出现在各种地方。

b.在IOS8之前的机器，scene场景清除掉以后里面的scheduler和action不再更新，而在IOS8中还会更新。**为什么？**

###解决方案：
a.如果加载时间（B场景存在时间）<TransitionFade动画执行时间，那么等待动画执行结束在进行跳转。

b.把场景中所有的元素的scheduler和action都停止。

	currScene:unscheduleUpdate()
    cc.Director:getInstance():getActionManager():removeAllActions()



