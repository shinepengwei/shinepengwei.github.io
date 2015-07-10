---
layout: post
title:  基于CBIR的商标检索
categories: 
- "编程"
- "项目"

---

<h1>代码及更详细说明下载地址：<a id="share_url" href="http://163.fm/XYKZAOP" target="_blank">http://163.fm/XYKZAOP</a></h1>
由于涉及到一篇正在准备发表的论文，请联系作者索要下载提取码。
<h1>一.项目研究目标</h1>
以商标库为模型，通过对CBIR相关算法的学习研究，了解图像处理、计算机视觉、模式识别等相关领域的最新研究成果，并与传统的数据库技术相结合，设计商标图像特征数据库，研究开发基于CBIR的商标检索系统。并通过进一步的系统测试和算法分析，为检索系统的进一步优化进行有益尝试。
<h1>二.主要研究内容</h1>
以商标库为模型，通过对基于内容图像检索关键技术的研究，开发商标检索系统，主要研究内容包括：

<strong>商标图像的特征提取和描述技术研究</strong>

图像特征的提取是基于内容的图像检索技术的基础。图像具有颜色、形状、纹理等多种特征，而对于不同图像各特征的表示能力有着较大差别，本项目首先要研究如何从原始商标图像中根据其特点提取具有较强表示能力的图像特征。

<strong>用户反馈算法</strong>

由于商标图像是人工图像，人的主观视觉判断相对图像整体感官很重要，需研究设计用户相关反馈算法从而减小用户与图像检索系统在主观理解上的偏差。

<strong>相似性度量</strong>

相似性度量是指图像特征间的相似性，是图像检索研究的重要组成部分。研究有效的相似性度量方法，从而满足实用的商标检索系统的检索性能和实时性需求。

<strong>系统的设计与开发</strong>

在对各类相关算法技术研究的基础上，采用面向对象的编程思想，开发商标图像检索系统。并通过对其性能的测试和分析，优化和改进系统性能。
<h1>三.相关算法研究及实现</h1>
<h2>图像边缘提取</h2>
在提取角点直方图特征之前，首先检测二值商标图像的边界像素点。目前提取边缘的常用算法是通过canny算子进行边缘检测，但是对于二值图像，唐振军等提出了一种更加简单快速的方法，且获得的边界同样是单像素点<sup>[1]</sup>。对于每一个目标像素点，判断其4邻域是否存在背景像素点，如果存在背景图像，即为边界像素。如果4邻域像素都为目标像素点，则此不是边界像素。如图1所示，对于目标像素P像素，如果像素P1、P2、P3、P4都为目标像素，则P像素不是边界像素。如果其中之一为背景像素，则像素P为边界像素。此算法较canny算法具有运算量小，效率高等优点，对于二值图像来说是一个较好的边缘提取算法。

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR1.png" alt="" />

图 1

如图2，图a是原图，图 b是边缘提取后的边缘像素图。实验证明，此算法提取的边缘像素是单像素点且没有断点出现。

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR4.png" alt="" /><img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR5.png" alt="" />

图 2二值图像边缘提取效果
<ol>
	<li>
<div>
<h2>图像特征提取算法</h2>
</div>
<h3>HU不变矩特征</h3>
图像的矩函数在模式识别、目标分类中得到了广泛的应用。1962年，Hu(Hu M.K.1962)提出了图像识别的不变矩理论，用7个Hu不变矩来表征图像的形状<sup>[1]</sup>。这些矩对于平移、缩放和旋转的变化不敏感,反映了区域的共有特性,已经广泛用于形状特性的描述。

对于离散的数字图像f(x,y)的p+q阶矩定义为：<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR6.png" alt="" />

其p+q阶中矩定义为：<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR7.png" alt="" />

式中，<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR8.png" alt="" />，<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR9.png" alt="" />表示图像的区域重心。中心矩表示了图像内不同灰度级的像素相对于其重心是如何分布的，因此中心矩具有位置无关性。为了获取针对图像缩放无关的性质，可以对该中心矩进行规格化操作，规格化后的中心矩表示为<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR10.png" alt="" />

式中，<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR11.png" alt="" />，<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR12.png" alt="" />中心规格矩对于物体缩放、平移和旋转均保持不变。基于规格化的二阶和三阶中心距，可以导出如下7个矩组：

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR13.png" alt="" />

上述七个不变矩组称为HU不变矩。HU M.K.证明了这个矩组中的<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR14.png" alt="" />~<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR15.png" alt="" />具有尺度、平移和旋转不变性；而<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR16.png" alt="" />只具有尺度和平移不变性，却不具有旋转不变性，仅在镜面对称时保持不变。
<ol>
	<li>
<div><strong>角点直方图
</strong></div></li>
</ol>
对于二值图像，其像素的排列结构可以更好的描述图像，因此，我们提出角点直方图表示图像边缘像素点的结构特征。角点直方图的主要思想是提取边缘像素点后利用边缘像素点8邻域内的邻像素点计算方向角度，从而建立角点直方图。设边缘提取后的图像上边界点序列为<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR17.png" alt="" />，p为像素点总数。每个点<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR18.png" alt="" />的8邻域内边缘像素点数量为N，当<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR19.png" alt="" />或<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR20.png" alt="" />时，认为点<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR21.png" alt="" />不是角点。当N=2时，认为<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR22.png" alt="" />是角点。角点的方向角度定义如下：

每个边缘像素点作为顶点，相邻的两个像素点作为边，所构成的夹角的角度就是角点的方向角度。角点的方向角度有四种：45<sup>o</sup>，90<sup>o</sup>，135<sup>o</sup>，180<sup>o</sup>。 每种方向角度对应多种情况：如90<sup>o</sup>：

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR23.png" alt="" /> <img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR24.png" alt="" /> <img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR25.png" alt="" /> <img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR26.png" alt="" />

再比如180<sup>o</sup>：

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR27.png" alt="" /> <img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR28.png" alt="" /> <img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR29.png" alt="" /> <img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR30.png" alt="" />

计算每一个角点的方向角度，然后统计每一个方向角度的角点数量，得到角点直方图。为了不受图像缩放影响，对直方图进行归一化处理：

B[i]=B[i]/S

其中B[i]为角点直方图，S为角点总数，即<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR31.png" alt="" />。

实验证明，45<sup>o</sup>，90<sup>o</sup>的角点数量普遍很小，不适于作为统计特征，因此，仅仅将135<sup>o</sup>，180<sup>o</sup>的角点数量作为特征值进行检索，特征值表示为<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR32.png" alt="" />
<ol>
	<li>
<div><strong>相似性度量及特征值归一化
</strong></div></li>
</ol>
特征提取后，通过计算两图像特征之间的欧氏距离来判断两图像的相似性。对于样本图像A和图像B，两幅图像的特征向量分别为<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR33.png" alt="" />和<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR34.png" alt="" />，其中n表示特征空间的维数。他们之间的欧氏距离定义为：

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR35.png" alt="" />

其中k表示权值，可以通过改变权值来改变每个特征的影响因子。

对于以上两种特征提取方法，每种特征均是一组特征值表示。由于每一个特征值元素代表的物理意义不同，他们的幅度也会有非常大的差异，如果直接计算其距离可能出现数值较小的特征被忽略，所以在计算欧氏距离前要将他们归一化<sup>[3]</sup>，从而消除这种偏差。

将N幅图像的某一特征分离表示为<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR36.png" alt="" />，首先计算他们的平均数<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR37.png" alt="" />和标准差<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR38.png" alt="" />：

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR39.png" alt="" />

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR40.png" alt="" />

然后利用下式将原序列归一化为N（0,1）分布的序列：

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR41.png" alt="" />

对于HU矩特征，他们每个图像的特征元素相差几个数量级，必须进行归一化。但是对于角点直方图特征，由于他们的特征元素的物理意义相同，所以可以不进行归一直接进行图像检索，经试验，和归一后效果没有显著差异。
<ol>
	<li>
<div><strong>综合特征检索
</strong></div></li>
</ol>
使用单特征描述图像往往具有片面性，检索效果不佳，解决的方法是使用多特征检索，通常有两种方法，特征融合和二次检索。

特征融合是通过对各特征值归一化后共同进行距离计算，从而进行特征匹配。本项目使用HU矩和角点直方图作为综合特征描述图像，图像A和图像B之间的相似距离可表示为：

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR42.png" alt="" />

其中，<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR43.png" alt="" />，表示图像归一化后的HU矩特征和角点直方图特征。

二次检索是首先根据一种特征进行初步检索，去掉大量明显与待查询图像明显不同的图像，使待选图像数减少，然后在剩下的图像中通过另一种特征进行检索。

集合<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR44.png" alt="" />表示数据库中的所有图像，n为数据库中商标图像数量。以特征值<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR45.png" alt="" />进行初次检索：

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR46.png" alt="" />

获得了m张最相似图像，然后根据特征<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR47.png" alt="" />进行二次检索：

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR48.png" alt="" />

最终得到k张最相似的图像。
<ol>
	<li>
<div><strong>用户反馈
</strong></div></li>
</ol>
由于用户和机器认知的区别，仅仅通过固定的检索模式无法检索出令人满意的效果。而用户反馈在一定程度上弥补了这种缺陷，使检索系统可以获得相对更好的检索效果。

用户的每次查询都希望能找到表达用户查询概念的最佳映射，一般是系统提供接口让用户自己设置特征权重，或者通过用户操作系统自动计算权重，从而达到用户反馈的效果。

基于修改特征权重的用户反馈算法的基本思想如下：首先由用户在检索到的图像中选取相似的图像，对于这些图像的特征分量进行分析。直观上讲，如果所有的相关图像在某个分量上非常接近，意味着该分量对应的特征值较好的反映出用户的查询，应该给予较高的权重。相反，如果所有相关图像在某个分量上的值彼此相差较远，则说明对应的特征无法很好的反映用户的需求，则其权重较小。

通过用户反馈调整权重算法如下<sup>[4]</sup>：
<ul>
	<li>
<div>从检索到的所有图像中找到相似的图像，为"正例"。</div></li>
	<li>
<div>求出所有正例图像的特征分量<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR49.png" alt="" />的标准差<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR50.png" alt="" />。</div></li>
	<li>
<div>令<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR51.png" alt="" />。</div></li>
	<li>
<div>将权重归一化，即<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR52.png" alt="" />。</div>
系统自动生成权值后，重新进行检索，即可得到用户反馈后的检索结果。</li>
</ul>
<ol>
	<li>
<div><strong>图像检索算法评价
</strong></div></li>
</ol>
目前对于图像检索算法的评价标准主要在对查找结果的准确性方面，而其他方面，如系统响应时间等，随着计算机硬件的发展，相对来说已渐渐成为无关因素。图像检索的主要评价标准是检索精度和检索回召率，它能够较好的反应检索算法的性能。用<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR53.png" alt="" />表示返回的相关图像数量，<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR54.png" alt="" />表示检索返回的所有图像数，<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR55.png" alt="" />表示数据库中所有的相关图像数。检索精度定义为检索返回的相关图像数量与检索返回的所有图像数的比率，即：

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR56.png" alt="" />

检索回召率表示检索返回的相关图像数量和数据库中所有的相关图像数量的比率，即：

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR57.png" alt="" />

如果单独使用检索精度和检索回召率中的一个，无法有效地评价算法。如果检索出来的图像越多，回召率就会越高，但检索精度会下降。同理，检索出来的图像较少就会得到较高的检索精度。因此，评价一个检索算法通常同时使用检索精度和检索回召率。以检索回召率为x轴，以检索精度为y轴，可以得到精度/回召率曲线<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR58.png" alt="" />，称为PVR曲线，曲线与x、y轴围成的面积为：

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR59.png" alt="" />

面积s(f)称为PVR指数。通常以PVR曲线和PVR指数作为评价算法标准。
<h1>四.图像检索系统</h1>
<ol>
	<li>
<div><strong>检索系统框架
</strong></div>
商标检索系统构架如图3所示：

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR60.png" alt="" /></li>
</ol>
图 3 商标检索系统框架
<ol>
	<li>
<div><strong>商标库的构建
</strong></div></li>
</ol>
选取了87张商标图片，并将其分别进行旋转、放大等处理，构成435张测试图片。然后选择一幅图像为测试图像，分别旋转15<sup>o</sup>、30<sup>?o</sup>、45<sup>?o</sup>、60<sup>?o</sup>、90<sup>?o</sup>、120<sup>?o</sup>、180<sup>?o</sup>、270<sup>?o</sup>，以及放大、缩小、平移等处理，获得了19张处理后的图像，也就是说，这幅图像在商标数据库中具有20张相关图像。

通过以上处理，共获得450张测试图像，作为商标库进行使用。并规定将这些商标图像放在D:\input文件夹下，以方便系统进行调用。

商标库如图4所示。

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR61.png" alt="" />

图 4 商标库
<ol>
	<li>
<div><strong>特征库的构建
</strong></div></li>
</ol>
使用SQL Server数据库，构建数据库cbir后创建表CBIR，在数据库里存储商标图像的相关内容。

数据库表如表1所示。

表格?1数据库表
<div>
<table border="0"><colgroup> <col /> <col /> <col /> <col /> <col /></colgroup>
<tbody valign="top">
<tr>
<td>属性</td>
<td>列名</td>
<td>类型</td>
<td>长度</td>
<td>备注</td>
</tr>
<tr>
<td>图像标识</td>
<td>Id</td>
<td>Int</td>
<td>4</td>
<td>自动增加</td>
</tr>
<tr>
<td>图像名</td>
<td>Imgname</td>
<td>Varchar</td>
<td>300</td>
<td>不允许空</td>
</tr>
<tr>
<td>图像宽</td>
<td>Width</td>
<td>Int</td>
<td>4</td>
<td>不允许空</td>
</tr>
<tr>
<td>图像高</td>
<td>Height</td>
<td>Int</td>
<td>4</td>
<td>不允许空</td>
</tr>
<tr>
<td>HU矩直方图</td>
<td>Hu0</td>
<td>Float</td>
<td>8</td>
<td>不允许空</td>
</tr>
<tr>
<td>HU矩直方图</td>
<td>Hu1</td>
<td>Float</td>
<td>8</td>
<td>不允许空</td>
</tr>
<tr>
<td>HU矩直方图</td>
<td>Hu2</td>
<td>Float</td>
<td>8</td>
<td>不允许空</td>
</tr>
<tr>
<td>HU矩直方图</td>
<td>Hu3</td>
<td>Float</td>
<td>8</td>
<td>不允许空</td>
</tr>
<tr>
<td>HU矩直方图</td>
<td>Hu4</td>
<td>Float</td>
<td>8</td>
<td>不允许空</td>
</tr>
<tr>
<td>HU矩直方图</td>
<td>Hu5</td>
<td>Float</td>
<td>8</td>
<td>不允许空</td>
</tr>
<tr>
<td>HU矩直方图</td>
<td>Hu6</td>
<td>Float</td>
<td>8</td>
<td>不允许空</td>
</tr>
<tr>
<td>角点直方图</td>
<td>Bor0</td>
<td>Int</td>
<td>4</td>
<td>不允许空</td>
</tr>
<tr>
<td>角点直方图</td>
<td>Bor1</td>
<td>Int</td>
<td>4</td>
<td>不允许空</td>
</tr>
<tr>
<td>角点直方图</td>
<td>Bor2</td>
<td>Int</td>
<td>4</td>
<td>不允许空</td>
</tr>
<tr>
<td>角点直方图</td>
<td>Bor3</td>
<td>Int</td>
<td>4</td>
<td>不允许空</td>
</tr>
</tbody>
</table>
</div>
创建数据库表的sql语句如下：
<div>
<table border="0"><colgroup> <col /></colgroup>
<tbody valign="top">
<tr>
<td><em>USE master
</em><em>go
</em><em>xp_cmdshell 'mkdir D:\DATA', NO_OUTPUT
</em><em>IF EXISTS(SELECT NAME FROM master.dbo.SYSDATABASES WHERE NAME=N'cbir')
</em><em>DROP DATABASE cbir
</em><em>GO
</em><em>CREATE DATABASE cbir
</em><em>ON
</em><em>(
</em><em>NAME=CBIR_DAT,
</em><em>FILENAME='D:\DATA\CBIR_DAT.MDF'
</em><em>)
</em><em>LOG ON
</em><em>(
</em><em>NAME=CBIR_LOG,
</em><em>FILENAME='D:\DATA\CBIR_LOG.LDF'
</em><em>)
</em><em>GO
</em><em>USE CBIR
</em><em>IF EXISTS(SELECT NAME FROM CBIR.dbo.SYSOBJECTS WHERE NAME=N'cbir')
</em>

<em>DROP TABLE cbir
</em>

<em>GO
</em>

<em>CREATE TABLE cbir/*院系*/
</em>

<em>(
</em>

<em>id int identity(1,1) NOT NULL,
</em>

<em>imgname VARCHAR(300) NOT NULL,
</em>

<em>width INT NOT NULL,
</em>

<em>height INT NOT NULL,
</em>

<em>hu0 FLOAT NOT NULL,
</em>

<em>hu1 FLOAT NOT NULL,
</em>

<em>hu2 FLOAT NOT NULL,
</em>

<em>hu3 FLOAT NOT NULL,
</em>

<em>hu4 FLOAT NOT NULL,
</em>

<em>hu5 FLOAT NOT NULL,
</em>

<em>hu6 FLOAT NOT NULL,
</em>

<em>bor0 FLOAT NOT NULL,
</em>

<em>bor1 FLOAT NOT NULL,
</em>

<em>bor2 FLOAT NOT NULL,
</em>

<em>bor3 FLOAT NOT NULL,
</em>

<em>)</em></td>
</tr>
</tbody>
</table>
</div>
<ol>
	<li>
<div><strong>图像检索系统操作说明
</strong></div></li>
</ol>
<ul>
	<li>
<div>在菜单栏点击"文件"-&gt;"打开.."，打开待检索的图像，打开后系统自动对图像进行相关处理和特征提取，并将图像及其特征值显示出来。</div></li>
	<li>
<div>在控制台特征选择下拉框选择图像检索算法，包括HU矩、角点直方图、特征融合和二级检索。默认为HU矩特征值检索。</div></li>
	<li>
<div>选择特征描述算法后，点击开始检索，右侧返回检索后的相关图像，并以相似性由高到低排列。点击左侧的"上一页"和"下一页"查看更多的检索返回图像。</div></li>
	<li>
<div>双击右侧检索到的图像在左侧可以看到此图像的相关信息。</div></li>
	<li>点击菜单栏"添加数据库"，将商标库（d:/input/）中的图像的特征值添加到特征数据库中。</li>
	<li>
<div>如果使用的检索算法是"特征融合"，可通过以下操作进行相关反馈从而获得更好的检索效果。</div>
<ul>
	<li>
<div>人工设置权值。</div>
<ul>
	<li>
<div>可在菜单栏选择"相关反馈-&gt;人工设置权值"设置所有的特征分量的权重。</div></li>
</ul>
</li>
	<li>
<div>系统根据用户操作，自动设置权值。</div>
<ul>
	<li>
<div>如果此图像与原图相似，则作为正例，点击"正例"添加。</div></li>
	<li>
<div>添加正例后可点击"相关反馈-&gt;机器学习"，系统自动根据用户选择的正例自动设置特征分量的权值。</div></li>
</ul>
</li>
</ul>
</li>
</ul>
<ol>
	<li>
<div><strong>检索系统界面
</strong></div></li>
</ol>
图像检索系统界面如图5所示。

其中，区域1显示待查询商标图像。区域2显示待查询商标图像的特征值。区域3控制选择检索算法。区域4显示检索到的结果图像。区域5控制显示结果图像。区域6显示选择的结果图像的相关信息。区域7控制选择结果图像中的正例。区域8显示选为正例的所有图像名称。

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR62.png" alt="" /><img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR63.png" alt="" /><img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR64.png" alt="" /><img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR65.png" alt="" /><img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR66.png" alt="" /><img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR67.png" alt="" /><img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR68.png" alt="" /><img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR69.png" alt="" /><img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR70.jpg" alt="" />

图5 图像检索系统界面
<h1>五.实验结果</h1>
为了验证相关算法性能，将图6作为测试图像，对其进行检索查询。

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR71.png" alt="" />

图6 测试商标图像

图7是对测试图像分别进行检索（分别基于HU矩、角点直方图、特征融合及二次检索）的检索返回结果，只显示出检索结果中的前20幅商标图像。

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR72.png" alt="" /><img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR73.png" alt="" /><img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR74.png" alt="" />
<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR75.png" alt="" />

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR76.png" alt="" />
<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR77.png" alt="" />

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR78.png" alt="" /><img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR79.png" alt="" />

图 7测试图像的四种检索结果

图8是选取检索得到图像中的所有正例（20个）后，进行用户反馈，重新基于特征融合进行检索的检索结果。

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR80.png" alt="" />

图 8 用户反馈后基于特征融合的检索结果

为了分析比较各检索算法性能和效果，常用评价方法是通过PVR曲线图比较各算法，图9是五种检索策略的PVR曲线图。

<img src="http://www.codingart.info/wp-content/uploads/2013/08/082913_1638_CBIR81.png" alt="" />

图 9 五次检索的PVR曲线图

由实验结果可见，角点直方图的单特征检索的准确率远高于HU矩，通过HU矩和角点直方图的特征融合和二次检索可以获得更好的检索效果。用户反馈后，检索效果与检索前相似，主要是因为使用的检索算法性能较差，但是从检索到的前10幅图像中可以看出，检索效果也有一定的进步。
<h1>参考资料</h1>
[1] 唐振军，张显全.一种二值图象边界提取算法[J].微计算机信息，2006，10-3

[2]M.K.Hu.Visual pattern recognition by moment invariants[J].IRE Trans InfTheory,1962,(8):179-187.

[3] 黄元元. 基于视觉特征的图像检索技术研究[D].南京：南京理工大学，2002.

[4] 王春媛. 基于内容的图像检索反馈算法研究[D].海尔滨：海尔滨工业大学，2006.
<h1>附录一 程序介绍</h1>
<ol>
	<li>
<div>软件介绍</div>
<ol>
	<li>
<div>软件使用的编程环境为vs 2008，vc6.0无法使用</div></li>
	<li>
<div>数据库使用的my sql，数据库平台可参考另一文档及Sql语句文件</div></li>
</ol>
</li>
	<li>
<div>图像类img（源图像表示用户选择用来检索的图像，待检索图像表示数据库中用来和源图像匹配的图像）</div>
<ol>
	<li>
<div>ImgDate?//此类存储位图的图像信息</div>
存储位图所有的信息，包括描述信息和图像内容数据</li>
	<li>
<div>ImgDib//此类表示图像的位图信息及图像特征。</div>
此类包括位图图像信息和图像特征。

图像处理操作和特征提取操作主要发生在此类</li>
	<li>
<div>Img//在系统中此类表示数据库总的待检索图像。</div>
主要存储数据库存储信息</li>
</ol>
</li>
	<li>
<div>特征计算模块character</div>
特征类基类：Character，主要存储图像特征相关信息

特征计算类：CharacterCaculater（继承character），增加特征计算函数

BorCharacterCaculater，HuCharacterCaculater两个特征计算类，继承CharacterCaculater，重写了caculate()函数</li>
	<li>
<div>数据库模块dao</div></li>
	<li>
<div>距离计算模块distanceCal</div>
此模块使用装饰类，如果需要计算多个特征的距离，每个类表示一个特征之间的距离，分别装饰计算。

计算距离类继承DistanceCalculater，重写覆盖calculateDistance()。</li>
	<li>
<div>图像类排序模块order</div>
使用方法：

om=GenerateFactory::generateOrderMethod(m_orderMethod);//设置检索方法

om-&gt;setImgList(m_imgList,m_imgCount);?//设置要排序的图像类数组

om-&gt;setDisMethod(m_character);//设置根据何种距离排序

if?(m_orderMethod == BINARIC)//如果是二次检索，要设置两种距离

{

om-&gt;setDisMethod(DISCAL_BOR);

om-&gt;setDisMethod2(DISCAL_HU);

}

om-&gt;orderTheImg();//排序</li>
	<li>
<div>工具综合模块util</div>
<ol>
	<li>
<div>对象工程类：根据参数生产对象。</div></li>
	<li>
<div>用户设置。</div></li>
</ol>
</li>
	<li>
<div>Mfc支持模块</div>
DOC：所有的模块使用方法都在这个类中。</li>
</ol>
总结：
<ul>
	<li>
<div>大部分代码有注释。</div></li>
	<li>
<div>大量使用了对象工厂模式和策略模式。对于每一模块有一个基类，所有的实现都是这个基类的子类。</div></li>
	<li>
<div>userConfigure.h里面记录了整个系统所用的环境信息。</div></li>
</ul>
<h1>附录二 Mysql数据库平台搭建</h1>
注：我是用的是mysql，但是其实信息比较简单，使用sqlite比较合适。甚至可以直接使用文本文件存储。当时主要是为了学习使用C++操作mysql。
<ol>
	<li><strong>安装mysql（mysql-essential-5.1.54-win32.zip）
</strong></li>
	<li><strong>安装Navicat(8.0.20)+Patch.rar（为了方便观察数据）
</strong></li>
	<li><strong>安装mysql-connector-odbc-3.51.27-win32.msi
</strong></li>
	<li><strong>配置DSN：
</strong></li>
</ol>

<strong>配置DSN</strong>Pasted from &lt;<a href="http://docs.noahweb.net/config_dsn.htm">http://docs.noahweb.net/config_dsn.htm</a>&gt;


1.点击"开始"--"设置"--"控制面板"。


2.选择"管理工具"。

3.选择"数据源（ODBC）"。

4.选择"系统DSN"选项卡。
<img src="../Local%20Settings/Temp/msohtmlclip1/01/clip_image001.jpg" alt="" />

5.点击"添加"按钮。
<img src="../Local%20Settings/Temp/msohtmlclip1/01/clip_image002.jpg" alt="" />

6.选中"MySQL ODBC 3.51 Driver",点击"完成"。

7.在"Data Source Name"后输入noahsample。

8.在"Data base Name"后输入noahsample,

9.在"User"后输入root。点击"Test Data Source"按钮，屏幕如下图所示，表示连接数据库成功。
<img src="../Local%20Settings/Temp/msohtmlclip1/01/clip_image003.jpg" alt="" />

10.点击"确定"，然后点击"OK"。屏幕如下图所示，表示数据源noahsample成功建立。
<img src="../Local%20Settings/Temp/msohtmlclip1/01/clip_image004.jpg" alt="" /></li>
</ol>



