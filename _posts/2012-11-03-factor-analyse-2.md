---
layout: post
title: 因子分析及EM的应用
category: "理论及算法"
tag: "机器学习"
---
<p style="margin: 0cm 0cm 0pt;">因子分析(factor analysis)是一种数据简化的技术。它通过研究众多变量之间的内部依赖关系，探求观测数据中的基本结构，并用少数几个假想变量来表示其基本的数据结构。这几个假想变量能够反映原来众多变量的主要信息。原始的变量是可观测的显在变量，而假想变量是不可观测的潜在变量，称为因子。</p>

<h1>1 问题</h1>
<p style="margin: 0cm 0cm 10pt;">在我们以前在混合高斯模型中使用EM算法时，都认为我们有足够数量的样本来找到高斯分布，比如一般认为训练样本数量m远大于样本空间维度n。但是如果n大于或等于m时，使用原来的方法就无法获得结果。比如，我们希望从数据中构建一个单高斯分布模型（比混合高斯分布要求更少的数据），通过使用以下公式获得高斯分布的参数：</p>
<p style="margin: 0cm 0cm 10pt;"><img src="/wp-content/uploads/2012/11/zrclip_003p7bad0537.png" alt="" width="328" height="140" /></p>
<p style="margin: 0cm 0cm 10pt;">计算发现，∑是奇异矩阵，因此就无法计算高斯分布的概率分布。因此，无法使用极大似然估计的方法构建高斯模型。</p>
<p style="margin: 0cm 0cm 10pt;">如下图，我们在二维空间上有两个样本，对其进行高斯建模后获得的模型就是一个非常长，非常窄的高斯模型，几乎就像一条直线。可以理解为高斯模型认为样本较大概率落在两点间的直线上。</p>
<p style="margin: 0cm 0cm 10pt;"><img src="/wp-content/uploads/2012/11/zrclip_004n4178f3.png" alt="" width="360" height="218" /></p>

<h1>2限制∑</h1>
<p style="margin: 0cm 0cm 10pt;">有上面的分析可以看出，无法构建高斯模型的原因主要是因为计算出来的∑是奇异的。那我们可以对其进行一些假设和约定，可能获得一些比较好的高斯模型。</p>
<p style="margin: 0cm 0cm 10pt;">我们可以约定∑是<strong>对角矩阵</strong>。也就是说，各特征之间互相独立，我们只需要计算各特征的方差即可，非对角线元素为0。</p>
<p style="text-indent: 36pt; margin: 0cm 0cm 10pt;"><img src="/wp-content/uploads/2012/11/zrclip_005p717dcba5.png" alt="" width="254" height="92" /></p>
<p style="margin: 0cm 0cm 10pt;">高斯分布的密度曲线是椭圆的，如果∑是对角矩阵，则这些椭圆的轴与横纵坐标平行。如下图所示</p>
<p style="text-align: center; margin: 0cm 0cm 10pt;"><img src="/wp-content/uploads/2012/11/zrclip_006n36f42959.png" alt="" width="299" height="216" /></p>
<p style="margin: 0cm 0cm 10pt;">我们进一步对∑进行限制，<strong>我们不仅要求</strong> <strong>∑</strong> <strong>是对角矩阵，还要求对角线上的元素相等</strong>，即∑=σI。</p>
<p style="text-indent: 36pt; margin: 0cm 0cm 10pt;"><img src="/wp-content/uploads/2012/11/zrclip_007p5e82fee.png" alt="" width="254" height="80" /></p>
<p style="margin: 0cm 0cm 10pt;">对于上面的那个例子，我们获得的密度曲线如下，密度曲线是一个圆形：</p>
<p style="text-align: center; margin: 0cm 0cm 10pt;"><img src="/wp-content/uploads/2012/11/zrclip_008nca08994.png" alt="" width="304" height="204" /></p>
<p style="margin: 0cm 0cm 10pt;">然后，对参数∑的限制同时代表着我们认为样本的不同特征向量之间是没有相关的互相独立的，但我们往往想从样本中发现不同特征向量之间的关联关系。</p>
<p style="margin: 0cm 0cm 10pt;">下面，我们将描述因子分析模型，该模型使用更多的参数来发现特征间的相关性，并且不需要计算完整的∑。</p>

<h1>3 边缘高斯分布和条件高斯分布</h1>
<p style="margin: 0cm 0cm 10pt;">在介绍因子分析模型之前，先介绍下多元高斯分布中的边缘分布和条件分布。</p>
<p style="margin: 0cm 0cm 10pt;">假设，随机变量向量x可分为两个向量：</p>
<p style="margin: 0cm 0cm 10pt;"><img src="/wp-content/uploads/2012/11/zrclip_009n23b10290.png" alt="" width="99" height="70" /></p>
<p style="margin: 0cm 0cm 10pt;">假设，x~N(u,∑),</p>
<p style="text-indent: 36pt; margin: 0cm 0cm 10pt;"><img src="/wp-content/uploads/2012/11/zrclip_010n7b765890.png" alt="" width="275" height="73" /></p>
<p style="margin: 0cm 0cm 10pt;">（推导过程不介绍了）结论：</p>
<p style="margin: 0cm 0cm 10pt;">多元高斯分布的边缘分布仍然是高斯分布：x1~N(u1, ∑11)</p>
<p style="margin: 0cm 0cm 10pt;">条件分布：x1|x2~N(u1|2, ∑1|2).</p>
<p style="text-indent: 36pt; margin: 0cm 0cm 10pt;"><img src="/wp-content/uploads/2012/11/zrclip_012p13a69ec5.png" alt="" width="259" height="66" /></p>

<h1>4 因子分析模型的思想和举例</h1>
<p style="margin: 0cm 0cm 10pt;">下面我们通过一个例子说明因子分析的思想。</p>
<p style="margin: 0cm 0cm 10pt;">在因子分析模型中，我们认为m个n维空间特征的样本x(i)(x1(i),x2(i),….xn(i))的产生过程如下：</p>
<p style="text-indent: -21pt; margin: 0cm 0cm 0pt 21pt; mso-add-space: auto; mso-list: l1 level1 lfo1;">l 首先在一个k(k&lt;n)维空间特征下产生m个样本：z(i)，同时z(i)~N(0,I)。</p>
<p style="text-indent: -21pt; margin: 0cm 0cm 0pt 21pt; mso-add-space: auto; mso-list: l1 level1 lfo1;">l 然后通过一个变换矩阵A(n*k维矩阵)，将z(i) 映射到n维空间，即A*z(i)。</p>
<p style="text-indent: -21pt; margin: 0cm 0cm 0pt 21pt; mso-add-space: auto; mso-list: l1 level1 lfo1;">l 然后在A*z(i)基础上加上均值μ（n维），即μ+A*z(i)。</p>
<p style="text-indent: -21pt; margin: 0cm 0cm 0pt 21pt; mso-add-space: auto; mso-list: l1 level1 lfo1;">l 在真实空间中，现实数据和模型相比往往具有一些误差和噪音，我们认为这些误差也是高斯分布。e~N(0,φ),u+A*z(i)+e</p>
<p style="text-indent: -21pt; margin: 0cm 0cm 10pt 21pt; mso-add-space: auto; mso-list: l1 level1 lfo1;">l 经过这些变换，才是我们的真实的样本x(i)=μ+A*z(i)+e</p>
<p style="margin: 0cm 0cm 10pt;">下面以图例解释上述步骤，假设我们有m=5个2维样本x(i)，即两个特征。</p>
<p style="text-align: center; margin: 0cm 0cm 10pt;"><img src="/wp-content/uploads/2012/11/zrclip_013p643d8030.png" alt="" width="248" height="204" /></p>
<p style="text-indent: -18pt; margin: 0cm 0cm 0pt 18pt; mso-add-space: auto; mso-list: l0 level1 lfo2;">1. 首先我们认为在1维空间，存在正态分布的5个z(i)。均值为0，方差为1。</p>
<p style="text-align: center; margin: 0cm 0cm 0pt 18pt; mso-add-space: auto;"><img src="/wp-content/uploads/2012/11/zrclip_014p4d087032.png" alt="" width="470" height="80" /></p>
<p style="text-indent: -18pt; margin: 0cm 0cm 10pt 18pt; mso-add-space: auto; mso-list: l0 level1 lfo2;">2. 然后使用某个A=(a,b)T将一维的z映射到2维，图形表示如下</p>
<p style="text-align: center; margin: 0cm 0cm 10pt;"><img src="/wp-content/uploads/2012/11/zrclip_015p162ae211.png" alt="" width="282" height="228" /></p>
<p style="text-indent: -18pt; margin: 0cm 0cm 10pt 18pt; mso-add-space: auto; mso-list: l0 level1 lfo2;">3. 加上μ(μ1,μ2)T，即将所有点的横坐标移动μ1，纵坐标移动μ2，将直线移到一个位置，使得直线过点μ，原始左边轴的原点现在为μ（红色点）。</p>
<p style="text-align: center; margin: 0cm 0cm 10pt;"><img src="/wp-content/uploads/2012/11/zrclip_016n36788320.png" alt="" width="242" height="191" /></p>
<p style="text-indent: -18pt; margin: 0cm 0cm 10pt 18pt; mso-add-space: auto; mso-list: l0 level1 lfo2;">4. 然而，显示和模型相比会有一定偏差，因此我们需要将上步生成的点做一些扰动（误差），扰动e~N(0, φ)。加入扰动后，我们得到黑色样本x(i)如下：</p>
<p style="text-align: center; margin: 0cm 0cm 10pt;"><img src="/wp-content/uploads/2012/11/zrclip_017p41cae4e8.png" alt="" width="282" height="203" /></p>
<p style="margin: 0cm 0cm 10pt;">其中由于z和e的均值都为0，因此μ也是原始样本点（黑色点）的均值。由以上的直观分析，我们知道了因子分析其实就是认为高维样本点实际上是由低维样本点经过高斯分布、线性变换、误差扰动生成的，因此高维数据可以使用低维来表示。</p>

<h1>5 因子分析模型</h1>
<p style="margin: 0cm 0cm 10pt;">根据上一节的解释，我们定义因子分析模型为隐含随机变量z经过随机变换和扰动得到的样本x，其中z就是因子，维度比x更低：</p>
<p style="text-indent: 36pt; margin: 0cm 0cm 10pt;"><a href="/wp-content/uploads/2012/11/111.png"><img class="aligncenter size-full wp-image-1071" title="111" src="/wp-content/uploads/2012/11/111.png" alt="" width="155" height="91" /></a></p>
<p style="margin: 0cm 0cm 10pt;">定义z和x的联合分布为联合高斯分布：</p>
<p style="margin: 0cm 0cm 10pt;"><img src="/wp-content/uploads/2012/11/zrclip_021n45e235a1.png" alt="" width="198" height="73" /></p>
<p style="margin: 0cm 0cm 10pt;">计算两个参数，（计算过程略）可知</p>
<p style="text-indent: 36pt; margin: 0cm 0cm 10pt;"><strong> <img src="/wp-content/uploads/2012/11/zrclip_022p4052419d.png" alt="" width="372" height="83" /> </strong></p>
<p style="margin: 0cm 0cm 10pt;">根据上式和我们对边缘高斯分布的定义和推导可知：x~N(u, AAT+ φ)。</p>
<p style="margin: 0cm 0cm 10pt;">因此，对于一个训练集，似然估计为</p>
<p style="margin: 0cm 0cm 10pt;"><img src="/wp-content/uploads/2012/11/zrclip_023n57535bcb.png" alt="" width="601" height="63" /></p>
<p style="margin: 0cm 0cm 10pt;">为进行最大似然估计，我们希望分别根据三个参数对上式求最大值。但是无法求上式的最大值（我反正不想试。。。）。因此使用EM算法。</p>

<h1>6 EM算法在因子分析的使用</h1>
<p style="margin: 0cm 0cm 10pt;">因子分析的原理理解了，如何去求解因子分析我认为不是最重要的，这里是使用EM进行求解。</p>

<table style="border-collapse: collapse; mso-padding-alt: 0cm 5.4pt 0cm 5.4pt; mso-border-alt: solid black .5pt; mso-border-themecolor: text1; mso-yfti-tbllook: 1184;" border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr style="mso-yfti-irow: 0; mso-yfti-firstrow: yes; mso-yfti-lastrow: yes;">
<td style="padding-bottom: 0cm; background-color: transparent; padding-left: 5.4pt; width: 442.8pt; padding-right: 5.4pt; padding-top: 0cm; mso-border-alt: solid black .5pt; mso-border-themecolor: text1; border: black 1pt solid;" valign="top" width="738">
<p style="margin: 0cm 0cm 0pt;">初始化三个参数；</p>
<p style="margin: 0cm 0cm 0pt;">循环重复直至收敛{</p>
<p style="margin: 0cm 0cm 0pt;">(步骤E)对于每一个i(样本)，计算：</p>
<p style="text-indent: 33pt; margin: 0cm 0cm 0pt; mso-char-indent-count: 3.0;"><img src="/wp-content/uploads/2012/11/zrclip_024p2163fcb4.png" alt="" width="309" height="47" /></p>
<p style="margin: 0cm 0cm 0pt;">(步骤M)针对三个参数最大化：</p>
<p style="text-indent: 33pt; margin: 0cm 0cm 0pt; mso-char-indent-count: 3.0;"><img src="/wp-content/uploads/2012/11/zrclip_025n7a0eeb9a.png" alt="" width="462" height="92" /></p>
<p style="margin: 0cm 0cm 0pt;">}</p>
</td>
</tr>
</tbody>
</table>
