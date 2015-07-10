---
layout: post
title:  unity之碰撞器
category: "游戏研发"
---

在Unity中，可以给物体添加碰撞器组件，即使用一张无形的网包裹住物体。碰撞检测可知道两个物体之间是否存在碰撞。

Unity中存在两种碰撞器：原型碰撞器和网格碰撞器。**原型碰撞器**表示使用简单的集合对象包裹住物体，如unity中的盒、球体、胶囊和车轮，这种碰撞器计算量较小，用处比较广泛。而**网格碰撞器**使用网格完全精确的包裹住物体，计算量较大，除非在对碰撞精确度要求较高的场合，一般不使用。

碰撞检测在Unity中有三种类型：

- 普通的碰撞检测：检测两个物体之间的碰撞。
- 触发器碰撞检测：检测特定碰撞器之间的碰撞。
- 光线投射：使用看不见的光线检测物体之间的碰撞。


#碰撞检测函数

玩家角色专用碰撞检测函数：OnControllerConlliderHit(ControllerConlliderHit);

此函数只能被包含着角色控制器组件的对象使用，用于获知玩家角色与哪一个物体发生了碰撞，碰撞的相关信息保存在ControllerConlliderHit中。

对于其他对象，碰撞检测使用其他函数：

触发信息检测：

1.MonoBehaviour.OnTriggerEnter( Collider other )当进入触发器
2.MonoBehaviour.OnTriggerExit( Collider other )当退出触发器
3.MonoBehaviour.OnTriggerStay( Collider other )当逗留触发器

碰撞信息检测：

1.MonoBehaviour.OnCollisionEnter( Collision collisionInfo ) 当进入碰撞器
2.MonoBehaviour.OnCollisionExit( Collision collisionInfo ) 当退出碰撞器
3.MonoBehaviour.OnCollisionStay( Collision collisionInfo )  当逗留碰撞器