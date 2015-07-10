


在action中，update是最重要的函数，此函数接受一个百分比参数，它表示动作的完成进度。update根据这个百分比将目标对象（可能是一个CCSprite对象，也可能是别的什么）做出相应的调整。
笔者经过统计发现，只有2种函数调用过update，一个是step，另一个就是update本身。在第一种情况中，step通过update来更新动作的表现，在第二种情况中，这多半是一个包含了其它动作的复杂动作（比如CCActionEase类系）。

CCAction * CCNode::runAction(CCAction* action)该函数调用CCActionManager::sharedManager()->addAction(action, this, !m_bIsRunning);此函数中调用了startWithTarget函数，将action注册到ActionManager中。

CCActionManager:update()函数中会执行每个action的step函数
virtual void step(float dt);该函数在每一帧被执行
如ActionInterval的step函数。
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
    
    this->update(MAX (0,                                  // needed for rewind. elapsed could be negative
                      MIN(1, _elapsed /
                          MAX(_duration, FLT_EPSILON)   // division by 0
                          )
                      )
                 );
}

step函数将当前已经过去的时间转为完成进度，然后调用action的update函数。

比如MoveBy的update函数如下，根据完成进度改变位置。

void MoveBy::update(float t)
{
    if (_target)
    {
        _target->setPosition(_startPosition + _positionDelta * t);
    }
}

http://www.cnblogs.com/cocos2d-x/archive/2012/03/06/2381696.html

http://www.cnblogs.com/cocos2d-x/archive/2012/03/13/2393898.html