---
layout: post
title: 回归算法
category: "理论及算法"
tag: "机器学习"
---
监督学习是根据输入的训练集学习获得一个假设函数，由特征映射到目标变量。主要解决两种问题，回归问题和分类问题，它们是根据目标变量是连续的还是离散的而分类的。
<p style="text-align: center;"><img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_112.png" alt="" /></p>

<h1>1 线性回归</h1>
对于线性回归，假设函数一般根据特征的个数而设置：

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_29.png" alt="" />（X0=1）

提出假设函数以后，只要根据训练集求出假设函数中的参数<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_32.png" alt="" />即可。

要求出参数，只要参数使训练集中的<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_42.png" alt="" />最接近y。提出：<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_52.png" alt="" />

根据训练集，只要求出最小J(?)。
<h2>LMS算法</h2>
LMS（least mean squares，known as Widrow-Hoff学习规则）算法是用来求出最小的J(?)，他的思想是梯度下降算法，即为了最小值，根据当前值向下降速度最快的方向移动。

注：梯度下降可能会找到局部最优点，但是由于J(?)是一个只有全局最优的凸函数，所以可以使用梯度下降算法。

给定一个初始? ，<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_62.png" alt="" />

假设训练集中只有一个元素：

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_72.png" alt="" />

则更新规则如下：

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_82.png" alt="" />

对于训练集存在多个元素的情况，有两种方法:

<strong>Batch gradient descent
</strong>

此方法在每一步遍历所有的训练集：

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_92.png" alt="" />

遍历轨迹如图所示：

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_102.png" alt="" />

<strong>Incremental gradient descent
</strong>

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_113.png" alt="" />

此方法在每一步中根据训练集中的一个元素，通常这种方法对于前一种方法更快，尤其在训练集较大的情况下。但此方法只能获得接近最小值的点，但是一般对于机器学习可以接受。
<h2>The normal equation</h2>
此方法不需要迭代求出J(?)的最小值。思想是令J(?)对于所有的?变量的导数都为0。

具体算法比较复杂，数学公式太多没看懂。
<h2>概率解释</h2>
此章从概率的角度验证了使用J(?)的合理性。其实我觉得我可以很好的理解或者说感觉J(?)是合理的，这里从另一个角度验证了，也是为了以后使用的概率模型做一个铺垫。

对于线性回归中使用的训练集，我们假设每个元素中由于种种原因会有一些误差，并且这些误差属于正态分布（高斯分布）。

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_122.png" alt="" />

则：

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_132.png" alt="" />

则，?的可能性为所有训练集中元素<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_142.png" alt="" />的可能性的乘积。

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_152.png" alt="" />

现在我们要使?的可能性最大，也就是求出?使L(?)最大

定义:

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_162.png" alt="" />

只要使上式最大，则L(?)也最大。

则只要使<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_172.png" alt="" />最小即可。
<h2>局部加权线性回归</h2>
在使用线性回归时，提出合理的假设函数很重要，也很难，因为很难确定参数的个数，特征个数确定，但是可以提出多次方程。这就会牵扯到过拟合和欠拟合的概念。

为了减小线性回归对参数假设的以来，可以使用局部加权线性回归。

此方法很简单，思想就是如果要求一个点x对应的结果y，只要根据x附近的训练集（已知数据）求出y。

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_182.png" alt="" />

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_192.png" alt="" />
<h1>2分类问题和逻辑回归</h1>
对于分类问题，结果变量y是几个离散的变量。

这里我们仅讨论y为0或1的情况，也就是只有两类的分类。
<h2>逻辑回归</h2>
在逻辑回归中，我们选择的假设函数为：

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_202.png" alt="" />

假设：

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_212.png" alt="" />

推出：

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_222.png" alt="" />

根据训练集，可以得出θ的概率为：

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_232.png" alt="" />

只要求出使L(?)最大化的?。

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_242.png" alt="" />

同样，使用梯度上升算法。

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_252.png" alt="" />

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_261.png" alt="" />

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_271.png" alt="" />
<h2>感知器算法</h2>
感知器算法是改变了g(z)函数，其他同上一节的一样。

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_281.png" alt="" />

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_291.png" alt="" />

感知器算法表面上同其他算法相似，但是感知器算法没有具体的概率意义，也没有概率最大化的评价算法。
<h2>牛顿算法</h2>
牛顿算法的引出是快速求出使y=0的x的方法。

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_30.png" alt="" />

对于f(?)只要迭代下式：

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_311.png" alt="" />

就能快速的求出f(?)=0。

而如果要求函数的最大值，只使函数导数为0。

即：<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_321.png" alt="" />

上述是对于?为变量，当?为矢量时，

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_33.png" alt="" />，<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_34.png" alt="" />

牛顿算法收敛速度极快，但是每一轮牛顿算法都要使用极大的计算量，因为要计算H矩阵，所以不是用与特征较多的情况。
<h1>3 广义线性模型</h1>
在解决以上两种问题的情况下，我们可以将以上两种模型推导出更广义的线性模型。

广义线性模型是一种概率模型。
<h3>指数分布族</h3>
<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_35.png" alt="" />

对于伯努利分布：

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_36.png" alt="" />

当<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_37.png" alt="" />，

则：

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_38.png" alt="" />

对于高斯分布：

令<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_39.png" alt="" />

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_40.png" alt="" />

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_411.png" alt="" />

由以上推倒可以，高斯分布和伯努利分布都是指数分布族的特例。
<h3>构建广义线性模型</h3>
说白了，<span style="color: red;">构建广义线性模型就是根据概率分布获得一个比较合适的假设函数</span>。

在构建广义线性模型之前，需要做三个假设：

1 假设y|x; ?属于指数族模型。

2 h(x)=E[y|x]。

3 <img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_421.png" alt="" />这个不太像一个假设，更像一个设计。

步骤：

1 首先根据概率公式求出指数族的相关参数。

2 根据参数和假设二求出h(x)的形式。

对最小二乘法：

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_43.png" alt="" />

对于逻辑回归：

<img src="http://www.codingart.info/wp-content/uploads/2012/09/091212_0238_44.png" alt="" />
