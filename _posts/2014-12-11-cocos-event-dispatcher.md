---
layout: post
title:  Cocos的事件分发
category: "游戏研发"
tag:  "Cocos"
---


EventListenerVector封装了两个数组保存listener
std::vector<EventListener*>* _fixedListeners;
std::vector<EventListener*>* _sceneGraphListeners;

##addEventListener

	void EventDispatcher::addEventListener(EventListener* listener)
	{
	    if (_inDispatch == 0)
	    {
	        forceAddEventListener(listener);
	    }
	    else
	    {
	        _toAddedListeners.push_back(listener);
	    }
	
	    listener->retain();
	}

_inDispatch表示正在分发的事件数量，在dispatcheEvent函数开始，存在代码DispatchGuard guard(_inDispatch);将_inDispatch+1
_inDispatch管理嵌套事件，即在一个事件中触发另一个事件，而_inDispatch保存当前嵌套的深度，其值为0表示没有事件在分发。

判断是否在分发消息，如果没有就把listener加到监听列表里面。
否则，放在临时数组。

void EventDispatcher::forceAddEventListener(EventListener* listener)
根据listener的ID去_listenerMap寻找是否有相应的EventListenerVector存放这种ID的listener，如果没有，就创建一个。然后将listener放进去。
_listenerMap:key为listenerID，value为一个EventListenerVector();

若侦听器优先级为0，则说明它是与场景对象绑定的，调用associateNodeAndEventListener函数，将节点与侦听器加入到_nodeListenersMap中。
associateNodeAndEventListener(node, listener);
_nodeListenersMap：key为node，value为vector<EventListener*>();

通过上述分析，我们可以进一步理解到EventDispatcher类内的几个侦听器列表变量的作用。
_listenerMap 用以侦听器类型（就是侦听器的ID)索引，值是一个数组，用来储存侦听同一侦听器ID的所有侦听器对象。
_priorityDirtyFlagMap 用来标记一类ID的侦听器列表是对象是否有变化，侦是侦听器ID，值为侦听级别。
_nodeListenersMap 用来记录结点类型数据的侦听器列表,通俗点说就是以结点为索引所有侦听的事件都存在这个map里面。

**addEventListenerWithFixedPriority**
addEventListenerWithFixedPriority函数添加一个固定优先级的EventListener。

这里强调了，0这个优先级不能被使用，因为这是显示对象侦听器优先级别。如果小于0的优先级那么这个侦听器事件会在画面渲染之前被触发，大于0的优先级会在显示对象渲染之后触发事件回调。

这个函数


**addEventListenerWithSceneGraphPriority**
将一个指定的事件侦听器依照场景图的优先级顺序加入到侦听器列表里面， 这个方法与场景图的绘制顺序有关系，

将一个listener以node节点的渲染优先级注册到事件分发器中。它的固定优先级为0，listener的在vector的顺序为<0, =0(scene graph), >0
void addEventListenerWithSceneGraphPriority(EventListener* listener, Node* node);

这两个函数类似，首先为listerner设置一些参数，然后调用addEventListener函数。



##注销侦听器

注销侦听器比较简单，
**removeEventListener(EventListener* listener)**
/** 根据侦听器的大类型来删除侦听器 */
void removeEventListenersForType(EventListener::Type listenerType);

/** 根据Node结点来删除侦听器. */
void removeEventListenersForTarget(Node* target, bool recursive = false);
 
/** 根据指定事件名称来删除侦听器 */
void removeCustomEventListeners(const std::string& customEventName);

/** 清除所有侦听器 */
void removeAllEventListeners();

注销操作比较简单，和添加侦听器相反，就是从那几个vector中删除listener

需要注意的是，在注销侦听器时，会判断_inDispatch是否为0（即是否处于事件分发阶段），如果处于事件分发阶段（>0），则并不从侦听器列表中马上删除侦听器，而是仅仅将listener设置setRegistered(false),在updateListeners函数中，删除isRegistered为false的侦听器。


##分发事件
下面我们介绍最重要的事件分发相关函数

事件分发函数为void dispatchEvent(Event* event);
这个函数不仅分发事件，还移除所有标志为要删除的侦听器。

	void EventDispatcher::dispatchEvent(Event* event)
	{
	    if (!_isEnabled)
	        return;
	    
	    updateDirtyFlagForSceneGraph();
	    
	    
	    DispatchGuard guard(_inDispatch);
	    
	    if (event->getType() == Event::Type::TOUCH)
	    {
	        dispatchTouchEvent(static_cast<EventTouch*>(event));
	        return;
	    }
	    
	    auto listenerID = __getListenerID(event);
	    
	    sortEventListeners(listenerID);
	    
	    auto iter = _listenerMap.find(listenerID);
	    if (iter != _listenerMap.end())
	    {
	        auto listeners = iter->second;
	        
	        auto onEvent = [&event](EventListener* listener) -> bool{
	            event->setCurrentTarget(listener->getAssociatedNode());
	            listener->_onEvent(event);
	            return event->isStopped();
	        };
	        
	        dispatchEventToListeners(listeners, onEvent);
	    }
	    
	    updateListeners(event);
	}

####1.updateDirtyFlagForSceneGraph，处理脏标志

####2.Touche事件单独处理：dispatchTouchEvent

####3.sortEventListeners：根据event的listenerId，对该id的所有listener排序。

sortEventListeners函数根据事件ID对listeners进行排序，这里有一个优化，查看脏列表这个id的listeners是否有过变化，如果存在变化重新排序。该函数调用sortEventListenersOfFixedPriority与sortEventListenersOfSceneGraphPriority两个方法。
此外，该函数会调用listeners->setGt0Index(index);函数，意义为标识listeners中getFixedPriority>=0的第一个元素的index

####4.对于侦听当前事件所有侦听器分发，dispatchEventToListeners(listeners, onEvent);这句话的作用为让listener执行onEvent函数。
onEvent函数执行listener的_onEvent函数，此函数就是我们注册的事件处理函数了。

如果在事件分发过程中，较高优先级的listener可以选择停止让事件继续分发，可以调用event的stopProgation函数，调用该函数后，event->isStopped()返回true。

```

	listener->_onEvent(event);
    return event->isStopped();

```
####5.updateListeners




	/** Updates all listeners
     *  1) Removes all listener items that have been marked as 'removed' when dispatching event.删除所有标识为removed的侦听器
     *  2) Adds all listener items that have been marked as 'added' when dispatching event.添加所有标识为added的侦听器
     */
	void updateListeners(Event* event);

1.onUpdateListeners定义了一个匿名函数，作用是清理_listenerMap 里的侦听器，无效的都会进行注销清除操作。
> 这里有个不明白的地方，为什么isRegistered为false的所有listener都要release掉
> 
> 答：在注销侦听器时，如果发现当前处于事件分发阶段，并不马上删除侦听器，而是将侦听器的isRegistered设置为false

2.如果_listenerMap中value为empty，删掉_priorityDirtyFlagMap以及_listenerMap中的相应元素。

3.将_toAddedListeners中的元素调用forceAddEventListener


##脏数据机制

在forceAddEventListener函数中，使用setDirety函数标识曾经改变过listenerId中的某一类侦听器。
	
	setDirty(listenerID, DirtyFlag::SCENE_GRAPH_PRIORITY);
	setDirty(listenerID, DirtyFlag::FIXED_PRIORITY);

在setDirety函数中，改变_priorityDirtyFlagMap，key=listenerID，value=flag
注意，这里的flag同时保存两种类型侦听器的脏标识，方法是使用位操作，两位分别表示两种侦听器的脏标识。



总结：

有经验的同学可以看出，其实这里用到了一个常用的设计模式就是观察者模式，采用了注册事件，触发采用回调函数来执行事件过程。

分发器将事件对象传递给在分发器里注册的事件侦听对象，根据事件类型做匹配，匹配到合适的侦听器后就算事件触发了，调用侦听器的回调函数来执行事件过程。

Cocos2d-x引擎中有一个分发器对象，就是在Direct类中的_eventDispatcher这个变量，在创建Direct对象时进行的初始化。

