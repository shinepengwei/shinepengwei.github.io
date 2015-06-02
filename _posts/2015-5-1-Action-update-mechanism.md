---
layout: post
title:  Action更新机制
category: "游戏研发"
tag:  "Cocos"
---

游戏中希望实现人物移动分为三个过程：加速、匀速、减速。而匀速又由于可能改变方向，匀速要分为多段。

实现的方式：每一段移动使用一个MoveBy，而多段移动的连接使用PerformWithDelay来控制开始时间。

example：
```
	hero:runAction(加速，1.0)
	performWithDelay(hero:runAction(匀速1，2.0),1)
	performWithDelay(hero:runAction(匀速2，2.0),1+2)
	performWithDelay(hero:runAction(减速，1.0),1+2+2)
```

出现了两个问题：

1.在每段移动结束到下一个移动开始，会有卡顿。

2.当performWithDelay的delayTime = 0时，卡顿明显。

原因如下：

1.使用performWithDelay，callback执行，action会在下一帧才产生影响，而这一帧上一段剩余的移动可能跑不足一帧，这一帧移动速度就会降低。

2.performWithDelay的delayTime = 0时，callback的action实际上会在第五帧才产生影响。

结论：

使用performWithDelay是不合理的，应该使用sequence连接action缓解卡顿。

更屌的结论：


个人认为如果写的人物移动等需要帧级精确的情况不应该使用Action，因为action的计算不是在逻辑更新中进行的,action在不同情况下可能会出现延迟一帧等情况。

这种要求每帧精确的，建议自己在逻辑里面控制。


##解决过程

cocos游戏循环：

1.用户输入。2.动画计算。3.物理模拟。4.逻辑更新。5.UI遍历、绘制。6.交换缓冲区。7.释放。


检测用户输入并执行的所有代码，都会在动画计算之前完成。

如果在逻辑更新中执行action`node:runAction(action)`，此帧并不会调用此action的step函数，因为action:step属于动画计算，逻辑更新时已经过了动画计算阶段，会在下一帧执行。

如果根据用户输入(比如用户点击)执行一个动画，那么此帧会调用一次action的step函数，因为动画计算阶段还没开始。


ActionInterval:step第一次不修改，不知道目的是什么。

```
void ActionInterval::step(float dt)
{
    if (_firstTick)
    {
        _firstTick = false;
        _elapsed = 0;
    }
    else
    {
        _elapsed += dt;
    }
    this->update(_elapsed );
}
```

###performWithDelay函数

```
function performWithDelay(node, callback, delay)
    local delay = cc.DelayTime:create(delay)
    local sequence = cc.Sequence:create(delay, cc.CallFunc:create(callback))
    node:runAction(sequence)
    return sequence
end

```

####PerformWithDelay(delayTime = 0)的问题。

```
	node:runAction(cc.MoveBy:create(1,cc.p(100,0)))
    print("actionBegin",self.time)
    performWithDelay(self,function() print("actionEnd",self.time)
        node:runAction(cc.MoveBy:create(1,cc.p(0,100)))
        end, 0)
```

第一帧：start MoveByX；start Sequence.

第二帧：MoveByX的第一次step；sequence第一次step，update(0) ，同时执行DelayTime的第一次update(0)。

>DelayTime虽然参数为0的action，但是参数为0在系统中其视为无穷接近0的正小数。


第三帧：x坐标修改。Sequence第二次step，正常的update。此时处于sequence第一个action完成，进入第二个action执行的帧次。第二个CallFun直接执行，start MoveByY

第四帧：MoveByY执行第一次step。

第五帧：y坐标修改。


输出：
```

------new zhen-------166
[LUA-print] actionBegin	2.0211729742587
[LUA-print] time	2.0211729742587
[LUA-print] self.testNode :	0	0
------new zhen-------167
MoveBy::update 0.000000
[LUA-print] time	2.0713009759784
[LUA-print] self.testNode :	0	0
------new zhen-------168
MoveBy::update 0.049765
[LUA-print] actionEnd	2.0713009759784
[LUA-print] time	2.1210659742355
[LUA-print] self.testNode :	4.9765000343323	0
------new zhen-------169
MoveBy::update 0.099745
MoveBy::update 0.000000
[LUA-print] time	2.171045973897
[LUA-print] self.testNode :	9.9744997024536	0
------new zhen-------170
MoveBy::update 0.149641
MoveBy::update 0.049896
[LUA-print] time	2.2209419757128
[LUA-print] self.testNode :	14.964100837708	4.9896001815796
------new zhen-------171
MoveBy::update 0.199535
MoveBy::update 0.099790
[LUA-print] time	2.2708359770477
[LUA-print] self.testNode :	19.953500747681	9.9790010452271

```



###Sequence:update源码解析

`_split`表示两个action中间的分界线`[0,1]`

`void Sequence::update(float t)`

t表示action当前的时间比例。

首先根据t以及`_split`确定执行哪一个action以及属于此action的时间比例`new_t`.

1.第一次进入update，此时found=0，last = -1

```
_actions[found]->startWithTarget(_target);
 _actions[found]->update(new_t);
```
 
2. 对于action1执行完action2要执行这一次update，此时found=1，last = 0,
- 执行完action1的最后一次并且stop`_actions[0]->update(1.0f);  _actions[0]->stop();`
- 开始执行action2,` _actions[1]->startWithTarget(_target);`
- 执行action1的update。`_actions[1]->update(new_t);`

3. 对于action1为CallFun这类ActionInstant，duration=0,这种情况，`_split = 0`,第一个found = 1，last = -1。
action1直接开始start、update、stop：
```
	_actions[0]->startWithTarget(_target);
	_actions[0]->update(1.0f);
	_actions[0]->stop();
```
然后此帧继续执行action2的start和update。

**注意，此帧会执行两个action的update，执行action1的update(1)，以及执行action2的正常的update**

比如两个MoveBy，那么这一帧两个MoveBy同时有效。

4. 对于action2为ActionInstant，`_split = 1`，最后一次update(1)，与情况<2>相同。

```
void Sequence::update(float t)
{
    int found = 0;
    float new_t = 0.0f;
    if( t < _split ) {
        // action[0]
        found = 0;
        if( _split != 0 )
            new_t = t / _split;
        else
            new_t = 1;

    } else {
        // action[1]
        found = 1;
        if ( _split == 1 )
            new_t = 1;
        else
            new_t = (t-_split) / (1 - _split );
    }
    if ( found==1 ) {

        if( _last == -1 ) {
            // action[0] was skipped, execute it.
            _actions[0]->startWithTarget(_target);
            _actions[0]->update(1.0f);
            _actions[0]->stop();
        }
        else if( _last == 0 )
        {
            // switching to action 1. stop action 0.
            _actions[0]->update(1.0f);
            _actions[0]->stop();
        }
    }
	else if(found==0 && _last==1 )
	{
		_actions[1]->update(0);
		_actions[1]->stop();
	}
    // Last action found and it is done.
    if( found == _last && _actions[found]->isDone() )
    {
        return;
    }
    // Last action found and it is done
    if( found != _last )
    {
        _actions[found]->startWithTarget(_target);
    }
    _actions[found]->update(new_t);
    _last = found;
}
```

###PerformWithDelay和Sequence的区别
首先来看通过perform连接MoveByX,MoveByY

    node:runAction(cc.MoveBy:create(1,cc.p(x,0)))
    print("actionBegin",self.time)
    performWithDelay(self,function() print("actionEnd",self.time)
        node:runAction(cc.MoveBy:create(1,cc.p(0,y)))
        end, 1)
    
MoveByX执行结束，这时候DelayTime也执行结束，此帧调用callback，MoveByY开始，此时还在动作计算阶段，因此执行第一次step，也就是update(0)。下一帧MoveByY产生影响。因此，这种情况是x执行完成，下一帧y产生影响

但是对于直接使用Sequence连接两个MoveBy：
	
	node:runAction(cc.Sequence:create(cc.MoveBy:create(1,cc.p(x,0)),cc.MoveBy:create(1,cc.p(0,y))))

当MoveByX执行完成那一帧时，直接执行MoveByY的update函数，MoveByY直接产生影响。

也就是说，有那么一帧，两个action同时产生影响。

>应该这么说，此帧已经到了第二个action的时间范围，那么就把第一个action执行完，然后执行第二个action。注意，此帧第二个action执行并不是像后面的时间跨度这么大。

>这样(使用sequence)是合理的。如果两个MoveBy都是X轴移动，而且移动速度相同，那么此帧就能根据时间结合两个X轴移动，且该帧的移动速度不变。但是如果使用perform，就会造成此帧只移动第一个action，而第一个action不到一帧的时间跨度（此时对于第一个action，t已经到了1.03，而只使用update(1.0)，因此这一帧移动速度变小，下一帧才按照第二个action的移动速度来。

```

------new zhen-------463
MoveBy::update 1.000000
MoveBy::update 0.002196
[LUA-print] time	3.0967550165951
[LUA-print] self.testNode :	100	0.21960735321045
------new zhen-------464
MoveBy::update 0.050128
[LUA-print] time	3.1446870155632
[LUA-print] self.testNode :	100	5.0128102302551

```