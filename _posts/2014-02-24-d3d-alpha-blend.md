---
layout: post
title: D3D-Alpha混合
category: "游戏研发"
tag:  "D3D"

---


在前面的介绍颜色的章节中，颜色结构体除了RGB信息外，还有Alpha信息，代表着物体的透明状态。当渲染一些如玻璃、湖水时，就可以通过Alpha混合，设置半透明物体的Alpha值和一个半透明计算公式把透明物体和后台缓冲区中的颜色混合，然后将混合后的颜色更新到缓冲区，实现了半透明物体的绘制。

##混合规则
Alpha混合规则可以用以下公式表示：
`COLOR=(src(RGB)*src(k)) OP (dst(RGB)*dst(k))`

- COLOR表示混合后的颜色
- src(RGB)表示源颜色值，即要回绘制的像素颜色
- src(k)表示源混合系数
- dst(RGB)表示目标颜色值，即当前后台缓冲区的颜色值。
- dst(k)表示目标混合系数
- OP表示源颜色计算值和目标颜色计算值混合的操作，一般为相加。
混合系数以及混合操作都通过`SetRenderState()`函数设置。

一般来说，将src(k)设置为`D3DBLEND_SRCALPHA`,将dst(k)设置为`D3DBLEND_INVSRCALPHA`,将OP设置为`D3DBLEND_ADD`,这样，公式就可以写为：

```
 COLOR=(src(RGB)*src(Alpha)) OP (dst(RGB)*(1-src(Alpha))
```

##Alpha混合使用方法
由于Alpha混合耗费较多的计算资源，默认状态下，Alpha混合功能时关闭的，要使用Alpha混合首先开启。

```
pd3dDevice->SetRenderState(D3DRS_ALPHABLENDENABLE, true);
```

根据上一节混合规则所说，我们要设置混合系数以及混合操作。
设置混合系数：

```
m_pd3dDevice->SetRenderState(D3DRS_SRCBLEND, XX);
m_pd3dDevice->SetRenderState(D3DRS_DESTBLEND, XX);
```

混合系数的枚举值可参考：http://msdn.microsoft.com/en-us/library/windows/desktop/bb172508.aspx

混合操作默认为相加`D3DBLEND_ADD`,也可以改为相减、最大值、最小值。

```
pd3dDevice->SetRenderState(D3DRS_BLENDOP, D3DBLEND_ADD);
```

##使用场景和实例
我们在雪花绘制的粒子系统中使用到了Alpha混合，当对雪花进行纹理贴图时，我们希望把纹理图的黑色边缘去掉，这时就可以使用Alpha混合。

雪花的贴图类似如下：

![](http://img.my.csdn.net/uploads/201304/01/1364751287_7662.jpg)

我们可以把源混合系数目标混合系数都设为一，因为黑色的RGB为000，混合以后黑色就变为透明。

```
	m_pd3dDevice->SetRenderState(D3DRS_ALPHABLENDENABLE, true);//打开Alpha混合
    m_pd3dDevice->SetRenderState(D3DRS_SRCBLEND, D3DBLEND_ONE);//源混合系数设为1
    m_pd3dDevice->SetRenderState(D3DRS_DESTBLEND, D3DBLEND_ONE);//目标混合系数设为1
	//TODO：渲染雪花
	m_pd3dDevice->SetRenderState(D3DRS_ALPHABLENDENABLE, false);//关闭Alpha混合	
```
