---
layout: post
title:  Direct3D雪人场景介绍
categories: 
- "游戏研发"
- "项目"
tag: "Direct3D"
---

通过对该次作业的完成，基本了解了direct3D的总体框架和相关技术，但是由于以前从未接触过，加上时间的关系，对有些技术细节和概念仍然理解的不够深刻，如多种灯光与材质、多层纹理混合的细节，投影变换的底层原理等。

总的来说，完成了作业的基本要求，并尝试挑战了更高的要求。
程序完成的功能有：

- 使用六面体，构建天空盒，其跟随摄像机的移动而移动。
- 基于高度图构建地形，并用贴图构建纹理。
- 检测用户输入，控制摄像机及视角的移动。
- 绘制一个球体作为太阳，其跟随摄像机的移动而移动。太阳随时间旋转移动，太阳移动的同时模拟出光照的变化。
- 使用x文件绘制雪人，除此以外还绘制了一个妹子在雪人旁边。
- 基于粒子系统，绘制雪花，使用Alpha混合去掉雪花图片的黑色边缘。
- 基于公告板技术，绘制树木，使用Alpha混合去掉树木图片的白色边缘。
- 显示操作说明文字，当太阳升起和降落的时候，动态显示提示文字。


需要进一步完善的功能：

- 阴影效果
- 碰撞效果
- 使用着色器shader



**程序的缺陷**

- 夜晚使用聚光灯效果，但只有雪人和妹子有光照效果，而地形只有环境光的效果。没有找到原因，还需要进一步研究。



##操作说明
程序通过鼠标和键盘操作（类似CS控制）：

- 按下鼠标，控制视角。
- W：向前移动
- S：向后移动
- A：左移
- D：右移
- R：垂直向上
- F：垂直向下


##程序架构
程序主要包括一下类和文件：

main.cpp：程序入口，初始化D3D设备和各模型数据，检测用户操作，显示操作说明信息，以及渲染场景。

Common.h：公用头文件，封装了安全release和delete的宏。

CameralClass:摄像机类，基于用户的操作控制摄像机的位置和观察向量，渲染前更新观察矩阵。

SunClass：太阳类，自发光体。模拟太阳的移动，随着摄像机的移动而移动。同时随着日出日落控制光照的变化并显示提示信息。

SkyBoxClass：天空盒类。构造一个六面体，使用纹理贴图，并随着摄像机的移动而移动

TerrainClass：地形类。使用高度图产生地形，使用贴图显示出地形的颜色。

SnowPointClass：雪花类。使用粒子系统技术构建雪花类。

BillboardClass：公告板类。场景中用于表示树木，通过公告板技术，使树木图片一直面向摄像机。

MeshClass：网格模型类。场景中表示雪人和妹子，类中可通过读取x文件，构建网格模型。


##程序入口文件 main.cpp

该文件为程序入口，文件中包括全局变量的定义，全局变量主要包括窗口句柄、d3d设备指针、场景中各个对象的指针，鼠标的位置信息和相关时间信息。

对于文件中函数，除了windows窗口初始化和消息句柄检测函数，最重要的就是d3d初始化函数init和渲染函数render。
在WinMain函数中，获取当前时间并计算与上一次渲染的时间间隔。
除此以外，还有两个函数，分为别：

void GetRealTimeUserInput(void);//检测用户输入，修改摄像机的位置和方向
void DrawHelpInfo(HWND hwnd);//输出左下角的操作提示信息
void init(void);//初始化
void shutDown(void);//关闭设备，清空对象内存
void render(void);//渲染绘制各个模型对象


##摄像机类-CameraClass
定义摄像机的位置和观察目标位置，根据用户的输入改变摄像机的位置和观察目标位置，从而达到用户控制视角的效果。

成员变量包括：

- d3d设备指针
- 摄像机的右向量和上向量：当摄像机左右移动或者上下移动时，根据这两个向量控制摄像机的位置。
- 摄像机的位置坐标和观察目标的位置坐标：用户控制会改变这两个坐标，在渲染前根据这两个坐标设置观察矩阵
- 摄像机的观察方向向量，为观察目标坐标-摄像机位置坐标。

成员函数：

- CameraClass(LPDIRECT3DDEVICE9 pd3dDevice, D3DXVECTOR3 vInitCameralPostion, D3DXVECTOR3 vInitTargetPostion)：初始化摄像机的初始位置和目标位置。
- MoveByMouse(float nXDiff,float nYDiff)：//根据鼠标的偏移量，改变观察向量和上向量
- PressKeyBord(unsigned char * keys)：根据用户的按键，调整视角的右、上和观察分量向量，用于更新观察矩阵
- UpdateViewMartix()：根据各个向量，计算观察矩阵
- getCamerPos()：返回摄像机位置，用于控制太阳、天空盒等。
- inLegalArea()：判断摄像机位置是否在合法区域，在键盘控制函数中使用。


##太阳类 - SunClass

成员变量：

- d3d设备指针。
- 太阳的网格模型 m_pSunMesh
- 太阳的材质 m_pSunMaterial

成员函数：

- Init()：初始化太阳，生成一个白色球体。
- Render(D3DXVECTOR3 vCameralPosition, float g_fTimeRanTime, HWND hwnd)：渲染太阳。太阳随着摄像机的移动而移动，同时根据程序运行时间移动，模拟日出日落概念，并且影响环境光照的明暗程度；根据日出日落，显示提示信息
- DrawInfo(LPCSTR info, HWND hwnd)：私有成员函数，在Render函数中使用，显示日出日落的提示信息。


##天空盒类 - SkyBoxClass

成员变量：

- d3d设备指针
- 六面体顶点缓存对象 m_pVertexBuffer
- 六面体6个纹理接口 m_pTexture[6]
- 六面体边长 m_Length

成员函数：
- Init(float Length)：天空盒初始化函数，根据天空盒长度填充顶点缓冲区数据
- LoadTextureFromFile()：从文件中加载六个面的纹理文件
- Render(D3DXVECTOR3* pCameraPos)：渲染天空盒,根据摄像机设定天空盒世界矩阵

##地形类

成员变量：

- d3d设备指针
- 地形的顶点缓存和索引缓存
- 地形的纹理 
- 地形的高度信息：存储float类型的vector
- 地形的属性，包括每行每列的单元格数和顶点数，地形的宽度、深度、缩放系数等。

成员函数：
- LoadTerrainFromFile(LPCSTR pRawFileName, LPCSTR pTextureFile)：从文件加载地形的高度图和纹理
- void Init(int nRows, int nCols, float fSpace, float fScale)：根据参数计算地形的相关属性，并且计算填充地形的顶点缓冲区和顶点索引缓冲区
- Render(D3DXVECTOR3 vPostion)：根据位置，渲染地形


##雪花类 - SnowPointClass

成员变量：
- d3d设备指针。
- 下雪区域和雪花数量
- 雪花粒子数组，该数组的元素保存的是雪花粒子的属性，定义为SNOWPOINT结构体，结构体包括位置、旋转角度、旋转速度、下降速度和纹理ID。
- 雪花粒子的顶点缓存，每个雪花粒子都是一个四边形贴图。
- 雪花纹理指针数组：一共找到了六个纹理文件，该数组元素保存指向纹理的指针

成员函数：
- void Init(float fAreaX, float fAreaY, float fAreaZ, int nSnowCount):粒子系统初始化函数，设置雪花粒子的属性数组、雪花顶点缓冲和雪花的纹理。
- Update( float fElapsedTime)：随着时间的流逝，更新雪花粒子的属性，主要包括模拟雪花的下降和雪花的旋转。
- Render()：渲染雪花粒子系统。期间使用Alpha混合技术，去掉图片背景的黑边。

##公告板类 - BillboardClass

成员变量：

- d3d设备指针
- 公告板顶点缓冲
- 公告板纹理

成员函数：
-Init(LPCSTR textureFileName): 初始化公告板，设置公告板纹理贴图，填充顶点缓冲区
- void BillboardClass::Render(D3DXVECTOR3 vPostion, D3DXVECTOR3 vCameralPos, float fScale):渲染公告板，基于公告板的位置和摄像机的位置设置世界矩阵，使公告板一直正对摄像机。

##网格模型类 - MeshClass

成员变量：

- d3d设备指针
- 网格模型对象
- 网格模型的材质信息
- 网格模型的纹理信息
- 网格模型的材质树木

成员函数：

- Init(LPCSTR xFileName)：初始化网络模型，读取x文件，填充网络模型对象的相关信息
- void Render(D3DXMATRIX *worldMat);//根据世界矩阵，



#附：
这一个月主要看了direct 3D相关的书籍和博客。

写的不太系统，因为是作为读书笔记写的，理解一部分写一部分，还有一些没有理解，还有一些技术如布告板等，还没有写。下面将继续看书写博客。

另外，理解3D渲染需要一定的数学基础，主要是线性代数。忘了许多，需要补起来。

Direct9 3D-初始化Direct3D设备

从顶点到几何体

索引缓冲

空间变换

纹理贴图

颜色与光照

粒子系统

Alpha混合

 

主要的参考书籍《Direct3D实时渲染技术》

博客http://www.codesampler.com/dx9src.htm 以及 http://blog.csdn.net/column/details/vc-game-programming.html?page=1