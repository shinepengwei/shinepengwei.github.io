---
layout: post
title: D3D-索引缓冲
category: "游戏研发"
tag:  "D3D"

---


上一节介绍了使用顶点构造几何体，在绘制复杂的几何体的时候，如一个正方体，很多顶点会重复使用多次。有的几何体需要构造多个顶点缓冲，如圆柱体需要一个侧面，两个底面的顶点缓冲。这样会带来性能和内存的浪费。

可以使用索引缓冲解决这个问题，索引缓冲的基本原理是为一个几何体只构造构造一个顶点缓冲，然后使用索引缓冲区索引顶点编号来表示几何体的多边形，编号（一个i额数字）所占用的内存远比一个顶点占用的内存少。

参考代码：http://www.codesampler.com/dx9src/dx9src_2.htm#dx9_indexed_geometry

##创建顶点缓冲区
比如，我们要创建一个正方体，如果使用普通的顶点缓冲，我们需要24（4*6，一个正方形使用4个定点，使用D3DPT_TRIANGLESTRIP图元）个顶点。而如果使用索引缓冲，在顶点缓冲区中只需要存储8个顶点。

这一步和上一节中的创建顶点缓冲区的方法相同。

```

	Vertex g_cubeVertices_indexed[] =
	{
		{-1.0f, 1.0f,-1.0f,  D3DCOLOR_COLORVALUE( 1.0, 0.0, 0.0, 1.0 ) }, // 0
		{ 1.0f, 1.0f,-1.0f,  D3DCOLOR_COLORVALUE( 0.0, 1.0, 0.0, 1.0 ) }, // 1
		{-1.0f,-1.0f,-1.0f,  D3DCOLOR_COLORVALUE( 0.0, 0.0, 1.0, 1.0 ) }, // 2
		{ 1.0f,-1.0f,-1.0f,  D3DCOLOR_COLORVALUE( 1.0, 1.0, 0.0, 1.0 ) }, // 3
		{-1.0f, 1.0f, 1.0f,  D3DCOLOR_COLORVALUE( 1.0, 0.0, 1.0, 1.0 ) }, // 4
		{-1.0f,-1.0f, 1.0f,  D3DCOLOR_COLORVALUE( 0.0, 1.0, 1.0, 1.0 ) }, // 5
		{ 1.0f, 1.0f, 1.0f,  D3DCOLOR_COLORVALUE( 1.0, 1.0, 1.0, 1.0 ) }, // 6
		{ 1.0f,-1.0f, 1.0f,  D3DCOLOR_COLORVALUE( 1.0, 0.0, 0.0, 1.0 ) }  // 7
	};
	g_pd3dDevice->CreateVertexBuffer( 8*sizeof(Vertex),
		                              D3DUSAGE_WRITEONLY,
									  Vertex::FVF_Flags,
		                              D3DPOOL_DEFAULT,
									  &g_pVertexBuffer_Indexed,
									  NULL );
	pVertices = NULL;
	g_pVertexBuffer_Indexed->Lock( 0, sizeof(g_cubeVertices_indexed), (void**)&pVertices, 0 );
	memcpy( pVertices, g_cubeVertices_indexed, sizeof(g_cubeVertices_indexed) );
	g_pVertexBuffer_Indexed->Unlock();


```

##创建索引缓冲区
在索引缓冲区内，存储的是顶点编号，使用CreateIndexBuffer函数创建索引缓冲。

```

	WORD g_cubeIndices[] =
	{
		0, 1, 2, 3, // Quad 0
		4, 5, 6, 7, // Quad 1
		4, 6, 0, 1, // Quad 2
		5, 2, 7, 3, // Quad 3
		1, 6, 3, 7, // Quad 4
		0, 2, 4, 5  // Quad 5
	};
	g_pd3dDevice->CreateIndexBuffer( 24*sizeof(WORD),//索引区长度
									 D3DUSAGE_WRITEONLY,//Usage
									 D3DFMT_INDEX16,//索引格式
									 D3DPOOL_DEFAULT,//Pool
									 &g_pIndexBuffer,//指向索引缓冲的指针
									 NULL );
	WORD *pIndices = NULL;

	g_pIndexBuffer->Lock( 0, sizeof(g_cubeIndices), (void**)&pIndices, 0 );
	memcpy( pIndices, g_cubeIndices, sizeof(g_cubeIndices) );
	g_pIndexBuffer->Unlock();

```
##通过索引缓冲绘制几何体
首先设置顶点缓冲，然后设置索引缓冲，然后设置FVF，最后通过DrawIndexedPrimitive函数绘制图形。
[DrawIndexedPrimitive函数官方文档](http://msdn.microsoft.com/en-us/library/windows/desktop/bb174369.aspx)

```

	HRESULT DrawIndexedPrimitive(
		[in]  D3DPRIMITIVETYPE Type,//图元类型
		[in]  INT BaseVertexIndex,
		[in]  UINT MinIndex,//被引用的最小索引值
		[in]  UINT NumVertices,//被引用的定点数
		[in]  UINT StartIndex,//索引到索引缓冲区内的顶点编号，从这里开始渲染。
		[in]  UINT PrimitiveCount//绘制图元的个数
	);


```

如绘制一个正方体

```
	g_pd3dDevice->SetStreamSource( 0, g_pVertexBuffer_Indexed, 0, sizeof(Vertex) );
	g_pd3dDevice->SetIndices( g_pIndexBuffer );
	g_pd3dDevice->SetFVF( Vertex::FVF_Flags );
	//绘制六个面。
    g_pd3dDevice->DrawIndexedPrimitive( D3DPT_TRIANGLESTRIP, 0, 0, 8,  0, 2 );
    g_pd3dDevice->DrawIndexedPrimitive( D3DPT_TRIANGLESTRIP, 0, 0, 8,  4, 2 );
    g_pd3dDevice->DrawIndexedPrimitive( D3DPT_TRIANGLESTRIP, 0, 0, 8,  8, 2 );
    g_pd3dDevice->DrawIndexedPrimitive( D3DPT_TRIANGLESTRIP, 0, 0, 8, 12, 2 );
    g_pd3dDevice->DrawIndexedPrimitive( D3DPT_TRIANGLESTRIP, 0, 0, 8, 16, 2 );
    g_pd3dDevice->DrawIndexedPrimitive( D3DPT_TRIANGLESTRIP, 0, 0, 8, 20, 2 );

```
##释放顶点缓冲和索引缓冲的资源
在程序结束前，需要使用Release释放资源，一般在D3D设备资源释放前。

