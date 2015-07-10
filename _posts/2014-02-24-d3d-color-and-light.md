---
layout: post
title: D3D-颜色与光照
category: "游戏研发"
tag:  "D3D"
  
---


##颜色
D3D中，颜色都是由红、绿、蓝三种颜色表示，并为了颜色定义了D3DCOLOR类型，D3DCOLOR是一个DWORD，由四部分构成，分别为Alpha和RGB，Alpha通道表示颜色的透明度。

可以通过`D3DCOLOR_ARGB(a,r,g,b)`生成一个D3DCOLOR颜色变量。


在定义顶点颜色的时候，根据FVF灵活顶点格式，我们只需要在定义顶点结构体格式时，定义颜色成员。

三角形的颜色由三角形的顶点颜色决定，而着色处理决定了如何通过定点的颜色决定三角形颜色。一般有两种着色模式，FLAT_SHADING和GOURAUND_SHADING。

- FLAT_SHADING模式：三角形内部所有像素的颜色都一样，都由第一个顶点的颜色决定
- GOURAUND_SHADING模式：根据三个点的颜色平滑。

设置着色模式的方法是使用`SetRenderState`，第一个参数设置为`D3DRS_SHADEMODE`

##基本光照模型
D3D关照模型包括四种光照效果：环境光、漫反射光、镜面反射光和自发射光。
###1.环境光
环境光模拟了场景中的全局辐射，没有位置和方向上的特征，只有一个颜色亮度，没有衰减，在所有方向和所有表面反射的环境光都是恒定不变的。

启用环境光的方法：`pd3dDevice->SetRenderState(D3DRS_AMBIENT,0xff888888)`，即设置了亮度为0xff888888的白色环境光。

###2.漫反射光
这种颜色是接收到从光源发射的光之后产生的，它与光源有关，与光线的照射方向和物体表面法线向量都有关系，还与物体的材质属性有关系。

通过设置`D3DRS_DIFFUSEMATERIALSOURCE`来确定漫反射的处理方式，可以取下列值：

- D3DMCS_MATERIAL：使用所定义的材质属性
- D3DMCS_COLOR1：使用顶点颜色中的漫反射属性值（default）
- D3DMCS_COLOR2：使用顶点颜色中的镜面反射属性值
- D3DMCS_FORCE_DWORD：一般不用

###3.镜面反射

可以通过`pd3dDevice->SetRenderState(D3DRS_SPECULARENABLE,TRUE)`开启镜面反射，一般不开启，因为计算量比较大。

###4.自发射光
自发射光指物体自身发射出来的光线，可以影响对象的颜色，但不能照亮其他物体。

##材质
物体的颜色由他的反射属性属性决定。*之所以一个球是红色的，因为他对红色的光放射的最多。*材质从本质上来说和顶点的颜色属性是一样的，但是在灵活顶点格式FVF中职能设置漫反射光和镜面反射光，而对于环境光和自发射光属性，必须通过材质属性设置。

材质的定义通过D3DMaterial9结构体

```

	ypedef struct D3DMATERIAL9 {
	    D3DCOLORVALUE Diffuse;
	    D3DCOLORVALUE Ambient;
	    D3DCOLORVALUE Specular;
	    D3DCOLORVALUE Emissive;
	    float Power;
	} D3DMATERIAL9, *LPD3DMATERIAL9;
```

Diffuse－指定表面反射的漫反射光。
Ambient－指定表面反射的环境光。
Specular－指定表面反射的镜面光。
Emissive－表面本身自发光。
Power －镜面高光，它的值是高光的锐利值，该值越大表示高光强度与周围亮度相差越大。

**注意，如果模型的顶点自己具有定点颜色属性，我们需要关闭定点颜色功能，这样使模型使用材质属性。**

```
	//关闭顶点颜色属性
	g_pd3dDevice->SetRenderState( D3DRS_COLORVERTEX, FALSE );

```

也可以通过设置光照计算使用我们设置的材质：

```
	g_pd3dDevice->SetRenderState( D3DRS_DIFFUSEMATERIALSOURCE, D3DMCS_MATERIAL );
    g_pd3dDevice->SetRenderState( D3DRS_SPECULARMATERIALSOURCE, D3DMCS_MATERIAL );
    g_pd3dDevice->SetRenderState( D3DRS_AMBIENTMATERIALSOURCE, D3DMCS_MATERIAL );
    g_pd3dDevice->SetRenderState( D3DRS_EMISSIVEMATERIALSOURCE, D3DMCS_MATERIAL );
```

通过以上两种方法，使物体基于我们定义的材质显示颜色。

如何应用材质，在绘制一个物体前，使用`pd3dDevice->SetMaterial(&p_material)`设置材质，然后绘制物体时就会使用这种材质。


##光源
在D3D中，光源表示对物体进行光照，物体根据自己的材质会产生相应的颜色。

一般对光源的处理步骤为：1.创建光源（即创建一个D3DLIGHT9结构体）。2.设置光源关闭或打开。3.绘制物体，这些物体会接受这些光源的照射而产生相应的颜色。

光源的信息包括位置、方向、强度等，内置3种光源类型：

- 点光源（Point Light）：有颜色和位置，没有方向，和电灯泡类似，随着距离延长而衰减。
- 方向光源（Directiongal Light）：就像太阳光一样，有方向、光强和颜色，没有光源位置，也不会衰减。
- 聚光灯（Spot Light）：类似现实生活中的探照灯或手电筒，有颜色、位置和方向。计算最为复杂。

第一步：定义D3DLIGHT9结构体，并设置光源并赋予其一个编号，表示渲染管道将加入该光源，D3D最多同时设置8个光源：

```

	typedef struct D3DLIGHT9 {
	  D3DLIGHTTYPE  Type;//光源类型
	  D3DCOLORVALUE Diffuse;//漫射光颜色
	  D3DCOLORVALUE Specular;//镜面光颜色
	  D3DCOLORVALUE Ambient;//环境光颜色
	  D3DVECTOR     Position;//光源位置
	  D3DVECTOR     Direction;//光照方向
	  float         Range;//光能传播的最大范围
	  float         Falloff;
	  float         Attenuation0;
	  float         Attenuation1;
	  float         Attenuation2;
	  float         Theta;
	  float         Phi;
	} D3DLIGHT9, *LPD3DLIGHT;

	如：
	D3DLIGHT9 light_0;
    ZeroMemory( &light_0, sizeof(D3DLIGHT9) );
    light_0.Type = D3DLIGHT_DIRECTIONAL;
    light_0.Direction = D3DXVECTOR3( 0.5f, -0.5f, 0.5f );
    light_0.Diffuse.r = 1.0f;
    light_0.Diffuse.g = 1.0f;
    light_0.Diffuse.b = 1.0f;
    g_pd3dDevice->SetLight( 0, &light_0 );//设置光源0

```

第二步：打开光源。渲染是，默认所有的光源都是关闭的，不起作用，如果使用需要启用他。`g_pd3dDevice->LightEnable( 0, TRUE );//启用0号光源`
第三步：开始绘制图形。这样，所有绘制的物体都会被光照0所照射。
