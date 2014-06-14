---
layout: post
title:  Unity Debug
category: "游戏研发"
---


1.使用动画，但是提示:
The AnimationClip 'TRun' used by the Animation component 'ThirdPerson' must be marked as Legacy.
UnityEngine.Animation:CrossFade(String)

解决方法：选择资源，在资源的Rig下面，将Animation Type 选择为Legacy

![](http://www.codingart.info/201405/unity-debug-animationClip.png)

参考：
http://answers.unity3d.com/questions/357851/animationclip-must-be-marked-as-legacy.html

原因：**不太懂**








