---

layout: post
title:  GLProgramState && GLProgram &&GLProgramCache && GLStateCache
category: "游戏研发"

---

GLPogramState保存GLProgram的状态（uniforms和attributes）。一个GLProgram可以被多个node使用，但是如果shader中使用uniform值，那么每个节点都需要拥有自己的GLProgramState。

每个node持有一个glState，每个glState对应一个GLProgram。

glState有一个GLStateCache保存所有的glState。key为GLProgram。

glProgramState有一个GLProgramCache保存所有的GLProgram，key为一个string类型的GLProgram name。


###不使用GLProgramCache产生的内存泄漏
泄露原因：GLStateCache泄露

过程：

使用createWithFileNames创建GLProgram。

创建了以后，使用setGLProgram，setGLProgram函数将去GLProgramStateCache根据glProgram创建或者获取一个glstate，node持有glProgramState指针，state指针持有它对应的glProgram。

每次进入场景，根据vsh和fsh创建新的GLProgram，使用这个GLProgram时，会创建新的glState并添加到GLProgramStateCache中。每次进入场景，都会讲glState添加到GLProgramStateCache中。

解决方法：使用GLProgramCache，每次创建GLProgram先去GLProgramCache中查找。

使用这种方法会出现新的BUG：多个node共用一个GLProgram，则多个node共用一个GLState，出现BUG。

###使用GLProgramCache，多个GLProgram共用一个state产生的BUG

![Alt text](/images/2015/1431345724510.png)

创建多个entity，他们共用一个state，发生错误。

读setGLProgram的源代码发现，使用GLProgramCache中相同的GLProgram创建state时获得的是相同的state。
```
	void Node::setGLProgram(GLProgram *glProgram)
	{
	    if (_glProgramState == nullptr || (_glProgramState && _glProgramState->getGLProgram() != glProgram))
	    {
	        CC_SAFE_RELEASE(_glProgramState);
	        _glProgramState = GLProgramState::getOrCreateWithGLProgram(glProgram);
	        _glProgramState->retain();
	    }
	}
	GLProgramState* GLProgramState::getOrCreateWithGLProgram(GLProgram *glprogram)
	{
	    GLProgramState* ret = GLProgramStateCache::getInstance()->getGLProgramState(glprogram);
	    return ret;
	}
```