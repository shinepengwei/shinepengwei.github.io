---
layout: post
title:  ExportJson格式动画文件多次读取问题
category: "游戏研发"
---


今天发现一个BUG：在进入某一个场景时，第一次进入人物动画显示，如果退出到初始界面再进入场景，人物就不显示了。而人物的显示是通过读取exportJson文件显示动画。

原因：使用addArmatureFileInfo读取exportJson文件时，没有显式说明对应的Plist和Png大图，而是用exportJson内部配置的默认Plist和PNG。

解决方法：使用addArmatureFileInfo三个参数的函数版本，告知其Plist和Png文件名。

建议：加载ExportJson文件时尽量使用三个参数的函数版本，同时告知Plist和Png文件名。

相关测试发现：场景第一次进入很卡，因为要加载资源。第二次进入不卡，因为加载过了资源，只需要从cache中读取。但是返回到初始界面在进入场景，一般都会卡，偶尔不卡。由此可见，返回初始界面对cache进行了清理。

分析exportJson动画文件的读取代码：

    void ArmatureDataManager::addArmatureFileInfo(const std::string& configFilePath)
    {
        addRelativeData(configFilePath);
    
        _autoLoadSpriteFile = true;
        DataReaderHelper::getInstance()->addDataFromFile(configFilePath);
    }
    void ArmatureDataManager::addArmatureFileInfo(const std::string& imagePath, const std::string& plistPath, const std::string& configFilePath)
    {
        addRelativeData(configFilePath);
    
        _autoLoadSpriteFile = false;
        DataReaderHelper::getInstance()->addDataFromFile(configFilePath);
        addSpriteFrameFromFile(plistPath, imagePath);
    }


两个函数相比，三个参数的函数多了下面一行代码：addSpriteFrameFromFile(plistPath, imagePath);

这行代码本质执行：SpriteFrameCache::getInstance()->addSpriteFramesWithFile(plistPath, imagePath);

分析addDataFromFile函数的源代码发现，在读取文件前函数首先检测是否添加filePath到ArmatureDataManager，如果添加过该ExportJson文件就不再添加。

因此该**问题的本质原因**为：ExportJson文件对应的图片（plist和png）保存在SpriteFrameCache中，当内存清理资源时（**IOS为什么清理，我也不明白**），只是把内存中的资源（SpriteFrameCache）进行了清理，但是保存加载过文件的数组没有清空。再次执行的时候，无法找到SpriteFrameCache中的图片文件。

-----
那么，为什么其他图片可以找到呢，比如背景图片等。

那是因为，其他图片保存在TextureCache中，而TextureCache找不到文件回去硬盘中再次去读取。

SpriteFrameCache和TextureCache的区别参考:
http://cocos2d-x.org/docs/manual/framework/native/v3/spriteframe-cache/zh




