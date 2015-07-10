---
layout: post
title:  cocos中SpriteFrameCache和TextureCache加载图片相关代码
category: "游戏研发"
tag:  "Cocos"
---

在SpriteFrameCache中，添加Plist大图到cache的函数提供了三个，分别如下：

```

	/** Adds multiple Sprite Frames from a plist file.
	 * A texture will be loaded automatically. The texture name will composed by replacing the .plist suffix with .png
	 * If you want to use another texture, you should use the addSpriteFramesWithFile(const std::string& plist, const std::string& textureFileName) method.
	 */
	void addSpriteFramesWithFile ( const std ::string & plist );

```

根据plist文件，添加SF。

寻找png方法：

首先去plist文件中查找textureFileName字段（一般都有）

如果没有将plist文件名加上png，作为textureFileName去添加。


```

	/** Adds multiple Sprite Frames from a plist file. The texture will be associated with the created sprite frames. */
	void addSpriteFramesWithFile ( const std ::string & plist , const std:: string & textureFileName );

```

该函数不去plist文件中查找textureFileName

```
	
	/** Adds multiple Sprite Frames from a plist file. The texture will be associated with the created sprite frames. */
	void addSpriteFramesWithFile ( const std ::string & plist, Texture2D * texture);
```

此函数根据一个已经生成的texture添加SF。


**总结：**

所有的addSpriteFramesWithFile都需要提供plist文件，区别只是提供texture的方式。

前两个函数通过自己的机制找到了texturePath以后，调用：

`Texture2D *texture = Director::getInstance()->getTextureCache()->addImage(texturePath.c_str());`

根据plist文件生成dict保存plist信息，三个函数最终都会调用`addSpriteFramesWithDictionary(dict, texture);`保存到SFcache中。

此外，SpriteFrameCache中，保存已经加载过的plist文件名_loadedFileNames，如果加载过就不再加载。

----

纹理Cache中下面的函数用于添加图片文件

Texture2D TextureCache::addImage(const std::string &path)

返回添加后的texture

首先去TextureCache中寻找fullpath为key的元素，如果存在说明这个textureFile已经添加过，就不再添加了。

```
	auto it = _textures.find(fullpath);
    if( it != _textures.end() )
        texture = it->second;
```