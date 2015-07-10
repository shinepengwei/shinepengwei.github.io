---
layout: post
title: NP问题
category: "理论及算法"

---
<h1 style="margin-left: 18pt;"><strong>预备知识
</strong></h1>
<p style="margin-left: 11pt;"><strong>判断/最优问题
</strong></p>
<p style="margin-left: 18pt;">有些问题的答案只有两种，yes or no。对于这种问题，称为判断型问题。还有些问题目的是求出最优解，比如最短路径等，称为最优型问题。为了简化NP问题的讨论，我们将所有的问题都限定在判断型问题上。这种限定是科学的，因为所有的最优问题都可转化为判断问题。</p>

<pre style="margin-left: 18pt;">Example：</pre>
<p style="margin-left: 18pt;">一个优化型问题定义如下：给定一个有向图G(V, E)以及V中两顶点s和t，找出一条从s到t的简单路径使得它含有的边最多。</p>
<p style="margin-left: 18pt;">把上述优化问题定义为一个判断型问题</p>
<p style="margin-left: 18pt;">假设(a)中的判断型问题有多项式算法A，请用算法A为子程序，设计一个多项式算法来解决对应的优化问题。</p>
<p style="margin-left: 18pt;">(a)我们引入一个变量k后，这个判断型问题可定义如下：</p>
<p style="margin-left: 18pt;">给定一个正整数k和有向图G(V, E)，以及V中两顶点s和t，是否存在一条含有至少k条边的从s到t的简单路径？</p>
<p style="margin-left: 18pt;">这个算法分两步，第一步确定最长的路径含有的边的个数k，第二步把这条最长路径找出来。做法是，对图中每一条边进行测试。如果把这条边刪去后，图中仍有一条长为k的路径，则将它刪去，否则保留。当每条边都测试后，剩下的边必定形成一条长为k的路径。</p>
<p style="margin-left: 11pt;"><strong>编码和语言
</strong></p>
<p style="margin-left: 18pt;">在计算机解决问题时，所有的问题都需要对问题进行编码表示。因此，对于一个判断性问题<span style="font-family: Symbol;">p</span>，我们可以使用x表示其编码。</p>
<p style="margin-left: 18pt;">反之，给定一个子符串x，可以有三种情况：(1) x代表问题<span style="font-family: Symbol;">p</span>的一个实例并且有答案yes；(2) x代表问题<span style="font-family: Symbol;">p</span>的一个实例并且有答案no；(3) x不代表问题<span style="font-family: Symbol;">p</span>的一个实例，只是一个杂乱的子符串而已。对第(1)种情况，我们用<span style="font-family: Symbol;">p</span>(x) = 1表示，而用<span style="font-family: Symbol;">p</span>(x) = 0表示另两种情况。</p>
<p style="margin-left: 18pt;">定义：给定一个判断型问题<span style="font-family: Symbol;">p</span>，它对应的语言L(<span style="font-family: Symbol;">p</span>) 是所有它的实例中有yes答案的实例的子符串编码的集合，即L(<span style="font-family: Symbol;">p</span>) = {x | x <span style="font-family: Symbol;">?</span>
<span style="font-family: Symbol;">S</span>* and <span style="font-family: Symbol;">p</span>(x) = 1}。</p>
<p style="margin-left: 18pt;"><strong>多项式归约</strong></p>
<p style="margin-left: 18pt;">给定两个语言L1和L2，如果存在一个算法f，它把<span style="font-family: Symbol;">S</span>*中每一个字符串x转换为另一个字符串f(x)，并且满足</p>

<ul style="margin-left: 39pt;">
	<li>x <span style="font-family: Symbol;">?</span>L1 当且仅当f(x) <span style="font-family: Symbol;">?</span>L2，</li>
	<li>f是个多项式算法，即转换在多项式时间内完成，</li>
</ul>
<p style="padding-left: 30px;">那么，我们说L1可多项式归约到L2，记为L1 <span style="font-family: Symbol;">￡</span>p L2，f称为多项式转换函数或算法。</p>
<p style="margin-left: 18pt;">如图14-2所示，转换函数f把<span style="font-family: Symbol;">S</span>*中每一个字符串映射到另一个字符串。注意，这个映射不要求单射(one to one)，也不要求满射(onto)，但一定要把L1 内的一个字符串映射到L2 内的一个字符，把L1 外的一个字符串映射到L2 外的一个字符串。</p>
<p style="margin-left: 42pt;"><img src="/wp-content/uploads/2012/08/081512_0540_NP11.png" alt="" /><span style="font-size: 18pt;">
</span></p>
<p style="text-align: center; margin-left: 36pt;"><span style="font-size: 10pt;"><span style="font-family: 宋体;"><strong>图1 </strong>多项式转换函数</span><em>f</em>必须分别<span style="font-family: 宋体;">把</span><em>L</em><sub>1</sub> 内和<em>L</em><sub>1</sub> 外的字符串映射到<em>L</em><sub>2</sub> 内和<em>L</em><sub>2</sub> 外
</span></p>

<h1 style="margin-left: 18pt;"><span style="font-size: 12pt;"><strong>P问题
</strong></span></h1>
<p style="margin-left: 18pt;">P语言类(class P)是所有可以被一个算法在多项式时间内判定的语言的集合，即P = {L | L <span style="font-family: Symbol;">í</span>
<span style="font-family: Symbol;">S</span>* 可被一多项式算法所判定}。如果语言L可以被一个算法在多项式时间内判定，那么L被称为属于P类的一个语言。</p>
<p style="margin-left: 18pt;">如果问题<span style="font-family: Symbol;">p</span>对应的语言L(<span style="font-family: Symbol;">p</span>)属于P类，那么问题<span style="font-family: Symbol;">p</span>也称为P类问题。</p>
<p style="margin-left: 18pt;">P问题代表着这个问题可以被确定性图灵机在多项式内解决。</p>

<h1 style="margin-left: 18pt;"><span style="font-size: 12pt;"><strong>NP问题
</strong></span></h1>
<p style="margin-left: 18pt;">NP问题是可以被非确定性图灵机在多项式时间内解决的问题。</p>
<p style="margin-left: 18pt;">非确定图灵机是一个理论模型，与确定性图灵机不同在于转换函数，确定性图灵机的转换函数是一对一的，而非确定性图灵机转换函数是一对多的。一个问题可以使用非确定型图灵机解决，我们可以将非确定性图灵机的状态路径树看作问题的遍历。</p>
<p style="margin-left: 18pt;">使用非确定性图灵机证明NP问题往往非常复杂，一般使用多项式检验机模型。</p>
<p style="margin-left: 18pt;">多项式检验机的输入除了问题的编码以外，还要求输入一个证书。证书是用来证明问题可以得到yes的回答。只要设计出了一个检验算法可以验证这个证书的正确性，那么这个问题就是NP问题。</p>
<p style="margin-left: 18pt;">例：证明判断有向图G(V, E)是否有哈密尔顿回路问题属于NP类。</p>
<p style="margin-left: 18pt;">证明：????如果G(V, E)有哈密尔顿回路，它通过每个顶点正好一次，那么我们可以把这个回路作为证书来验证。我们用p表示有n个顶点的序列并作为输入的证书。多项式检验算法的伪码如下：</p>
<p style="text-align: justify; margin-left: 18pt;"><span style="font-size: 10pt;"><strong>Hamilton-Cycle-Verification </strong>(<em>G</em>(<em>V</em>, <em>E</em>), <em>p</em>)
</span></p>

<ol style="margin-left: 54pt;">
	<li>
<div style="text-align: justify;"><span style="font-size: 10pt;">检查是否有|<em>p</em>| = |<em>V</em>|
</span></div></li>
	<li>
<div style="text-align: justify;"><span style="font-size: 10pt;">检查 <em>p</em> 中每个顶点是否属于集合 <em>V</em>
</span></div></li>
	<li>
<div style="text-align: justify;"><span style="font-size: 10pt;">检查<em>V</em>中每个顶点是否在<em>p</em>中出现
</span></div></li>
	<li>
<div style="text-align: justify;"><span style="font-size: 10pt;">检查从<em>p</em>中每个顶点到下个顶点是否是<em>E</em>中一条边
</span></div></li>
	<li>
<div style="text-align: justify;"><span style="font-size: 10pt;">检查从<em>p</em>的最后一个顶点到<em>p</em>的第一个顶点是否是<em>E</em>中一条边
</span></div></li>
	<li>
<div style="text-align: justify;"><span style="font-size: 10pt;">如果第1步到第5步的答案都是<em>yes</em>，那么输出1，否则输出0
</span></div></li>
	<li>
<div style="text-align: justify;"><span style="font-size: 10pt;">End
</span></div></li>
</ol>
<p style="padding-left: 30px;">?这个模型和非确定性图灵机在本质上是相同的。因为在非确定性图灵机中，我们有很多状态路径，如果问题有一个解，那么这个问题就是NP问题。那么我们假设已经找到了这个解作为证书，那么我们就可以使用确定性图灵机来判断这个证书。</p>
<p style="margin-left: 18pt;">其实，我们在现实中遇到的问题几乎全部都是NP问题，非NP问题只在理论上存在，实践中除非复杂性计算方面的科研项目才可能遇到。所以，一般问题都是NP问题，证明只是为了更加严谨。</p>

<h1 style="margin-left: 18pt;"><span style="font-size: 12pt;"><strong>NPC问题
</strong></span></h1>
<p style="margin-left: 18pt;">NPC问题是NP问题中最难的问题，所有的NP问题都可以多项式归约到NPC问题，而且NPC问题之间也可以互相归约。</p>
<p style="margin-left: 18pt;">目前为止，人们还不知道是否有一个NPC语言L可以被一多项式算法所判定，也不能证明任何一个NPC语言L不可能被一多项式算法所判定。因此，如图14-4所示，集合P，NP，和NPC的关系有两种，但大部分人相信第二种(图14-4(b))，但有待证明。</p>
<p style="margin-left: 18pt;"><img class="aligncenter" src="/wp-content/uploads/2012/08/081512_0540_NP21.png" alt="" /><span style="font-size: 10pt;">
</span></p>
<p style="text-align: center; margin-left: 18pt;"><span style="font-size: 10pt;"><strong>图2</strong> 集合P、NP、和NPC的两种可能的关系
</span></p>

<h1 style="margin-left: 18pt;"><span style="font-size: 12pt;"><strong>NPC问题证明
</strong></span></h1>
<p style="margin-left: 18pt;">根据NPC的定义，我们如果要证明一个问题是NPC，首先要证明这个问题是NP问题，然后证明所有的NP问题都可以归约到这个问题。</p>
<p style="margin-left: 18pt;">证明NP问题比较简单，但是证明所有的NP问题都可以归约这个问题相对比较复杂。但是，我们可以利用已知的NPC问题，因为所有的NP问题都可以归约到已知的NPC问题，如果已知的NPC问题可以归约到新的问题，那么新的问题也是NPC问题。</p>
<p style="margin-left: 18pt;">利用已知NPC问题证明新的问题是NPC问题的思想如下：</p>
<p style="margin-left: 18pt;">找到新问题对应的判定性问题，将已知的NPC通过某种变化变成新问题的某一特例，并保证已知NPC问题成立当当且仅当新问题成立。</p>
<p style="margin-left: 18pt;">将已知NPC问题变为新问题特例没有一个通用的方法，找到这种方法需要一些技巧，也需要一些经验甚至灵感。</p>
<p style="margin-left: 18pt;"></p>
