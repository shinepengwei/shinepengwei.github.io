---
layout: post
title:  IOS中的memory warning处理
category: "游戏研发"
tag:  "IOS"
---


游戏中在内测阶段出现了一些很奇怪的问题，这些问题在开发阶段从来没有遇到过，主要表现在创建一个sprite创建失败。刚开始我们也一头雾水，因为创建这些sprite的时候内存使用很低，有的才几十M，按说不应该出现内存问题，后来测试发现确实是ios系统发送给游戏memory warning，调用`didReceiveMemoryWarning`函数，cocos中处理memory warning的默认方法是调`void Director::purgeCachedData(void)`函数，此函数的主要工作是将内存中没有使用的texture和spriteFrame删掉。

```
	SpriteFrameCache::getInstance()->removeUnusedSpriteFrames();
    _textureCache->removeUnusedTextures();
```

当删除了这些spriteFrame和texture之后，在后面如果使用spriteFrame创建sprite，就会创建失败。

使用cocos默认的memory warning处理方式有以下问题：
1. 删除一些与加载了而后面需要使用的spriteFrame和texture，造成BUG。
2. 两个spriteFrame对应相同的texture，其中一个使用了，一个没使用，这时候texture无法remove，只能remove那个没使用的spriteFrame信息，其实释放的内存可以忽略。

一个好的memory warning处理方式应该有一些部分：
- 一个unUsed的spriteFrame如果它对应的texture无法被remove，那么它不应该被remove，（因为对内存基本没有影响）
- 当使用spriteFrame创建一个sprite的时候，应该去判断这个spriteFrame是否在cache中，如果不存在，还需要载入内存后创建。（可以写一个接口，cc.Sprite:createOrReloadWithSpriteFrame()）

另外，IOS产生memory warning的信息并不是说游戏占用内存达到某个固定阈值产生内存警告，而是对于不同的手机环境（硬件、后台打开的程序、电话等）会在不同的阈值产生内存警告，比如我们游戏在50M左右就出现过内存警告。

IOS中的内存警告分为几个等级，一般达到某个等级系统会直接杀死程序（闪退），但是低等级的内存警告并不是说游戏一定要释放内存。也就是说，即使内存警告出现，游戏根据自己的逻辑释放很少量甚至直接忽略内存警告也是可以的。

内存警告的意义在于：系统告知游戏内存占总体的资源比例过多，删掉不使用的内存。但是如果我们的游戏没有可以删除的内存资源，就只能忽略掉这个警告，即使系统一直产生低等级内存警告，并不会杀死游戏进程。