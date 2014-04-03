---
layout: post
title: D3D-空间变换
category: "游戏研发"
tag:  "D3D"

---

计算机屏幕是平的，如何在平面上显示出来3D效果，就需要一系列的空间变换，他们分别是世界变换、视口变换和投影变换。

把物体的数学描述以及与物体相关的颜色信息转换为屏幕上的像素，这个过程称为光栅化。

##世界变换
物体在建立的时候是在自己的坐标系中建立的，物体建立以后需要放在世界坐标系中，这个过程就是世界变换，简单地说就是把物体放在他们应该放的地方。

世界变换通过平移、旋转和缩放矩阵变换来实现，这些矩阵一般是一个`4*4`的矩阵，世界变换就可以理解为对一个物体的顶点基于这个`4*4`的矩阵进行处理。

基本的步骤为：

1. 根据想要实现的功能设置并赋值一个世界矩阵。
2. 使用SetTransform设置世界矩阵。
3. 绘制物体。

例如，我们要将一个物体绕着X轴旋转angleRotateX角度，然后再平移(x,y,z)，可以写程序如下


```

	D3DXMATRIX matRotatioin;
	D3DXMATRIX matTranslation;
	D3DXMATRIX matCylinder;
	D3DXMATRIXIdentity(&matCylinder);//将matCylinder设置为单位矩阵
	D3DXMATRIXTranslation(&matTranslation,x,y,z);//移动X,y,z
	D3DXMATRIXMultiply(&matCylinder,&matCylinder,&matTranslation);
	D3DXMATRIXRotationX(&matRotatioin,angleRotateX);
	D3DXMATRIXMultiply(&matCylinder,&matCylinder,&matRotatioin);

	g_pd3dDevice->SetTransform(D3DT_WORLD,&matCylinder);

	g_pSunMesh->DrawSubset(0);

```

##视口变换
世界变换定义了物体在世界空间的位置，但是需要确定眼睛（摄像机）的位置，才能知道物体如何在一个平面显示出3d效果。

视口变换就是通过确定一个摄像机位置，将世界空间坐标转为视口空间，即屏幕看到的空间。


D3D中，使用D3DXMatrixLookAtLH函数构建视口矩阵，该函数原型如下：
```
	D3DXMATRIX *WINAPI D3DXMatrixLookAtLH(          
		D3DXMATRIX *pOut,//输出的视口矩阵
	    CONST D3DXVECTOR3 *pEye,//眼睛所在的位置
	    CONST D3DXVECTOR3 *pAt,//观察目标位置向量
	    CONST D3DXVECTOR3 *pUp//当前世界坐标系上方的向量，一般设置为(0,1,0)
	);
```

构建视口矩阵以后，就和世界变换类似，使用`g_pd3dDevice->SetTransform( D3DTS_VIEW, &matView );`进行视口变换。

##投影变换
投影变换根据物体和摄像机的距离，产生有层次感的画面。

可以理解为对摄像机的特性进行设定，如视野范围（角度）、近平面和远平面。

一般使用D3DXMatrixPerspectiveFovLH函数。

```
	D3DXMATRIX * D3DXMatrixPerspectiveFovLH(
		
		D3DXMATRIX *pOut,//投影变换矩阵
		FLOAT fovy,//视野角度
		FLOAT Aspect,//视口的宽高比
		FLOAT zn,//近平面距离		
		FLOAT zf//远平面距离
	
	);

```

获得了投影变换矩阵以后，使用`g_pd3dDevice->SetTransform( D3DTS_PROJECTION, &mProjection );`进行投影变换。


<hr/>

投影变换以后，有的顶点对于屏幕处于不可见状态，光栅化程序也就不需要计算他的颜色和阴影，成为裁剪。



