---
layout: post
title:  编程之美 读书笔记
category: "理论及算法"
tags:
- "读书笔记"
- "编程之美"

---

<div><strong>2.1求二进制数中1的个数</strong></div>
<div>解法一：将数除以2至0，余数为1的次数数</div>
<div>解法二：使用位运算：将该数与0x01与运算，然后左移1位。与运算结果为1的次数。位操作和除、余操作相比效率更高。</div>
<div>解法三：让算法的复杂度只和1有关，原理是v&amp;(v-1)后的结果即为将v二进制数的为1的最低位消除。</div>
<div>解法四：使用分治判断法判断一个数的1的位数，但是根据程序的书写最多可能要判断255此才可以，因此此方法不太好。</div>
<div>解法五：使用分治判断不好，那可以使用hash将二进制数(key)映射到1的个数(value)，只要前期将hash表建好即可。这种方法是使用空间换时间，若二进制位数较少如八位，这个是很好的算法，但是若位数很多如32位，这个方法就不好了。</div>
<div>拓展问题：两个二进制整数A、B，他们有多少位是不同的？</div>
<div>答：异或操作，结果然后计算1的个数。</div>
<div>

<hr />

<strong>2.2 不要被阶乘吓到</strong></div>
<div></div>
<div></div>
<div>

<hr />

</div>
<div><strong>2.3寻找水王</strong></div>
<div><span style="color: #939600;">一个ID发的帖子超过了所有帖子总数量的一般，如何找到这个ID。</span></div>
<div>解法一：对所有帖子的ID排序，然后找到中间的那一个ID即可。需要排序，时间复杂度：O(N*lgN)</div>
<div>解法二：每次删掉两个不同的ID，最后剩下的就是要找的ID。只需要遍历一遍，O(N)即可。</div>
<div><strong>代码也很巧妙</strong>，遍历ID列表，找到一个ID作为candidate，设置一个time来保存这个ID出现的次数。继续遍历，如果新的ID和这个ID相同，time加以。如果不同，time-1（这个行为就是删掉两个不同的ID）。如果time为0，选择新的candidate。</div>
<div>扩展问题：有三个发帖很多的ID，每个人的发帖数目超过了全部的1/4，如何快速找到这3个ID？</div>
<div>同理，每次删掉四个不同的ID，最后剩下3个ID即可。</div>
<div>很多问题都可以用这种思想解决：考虑如何把一个问题转换成规模较小的若干问题。分治、递推和贪心都是这样的思路。转化过程中，小的问题跟原问题本质上一致。如这个问题，我们保证了问题的解在小问题中仍然具有和原问题相同的性质：水王的ID在ID列表中的数量超过一半。</div>
<div>

<hr />

<strong>2.4 1的数目</strong></div>
<div></div>
<div><strong>2.5 寻找最大的K个数</strong></div>
<div>解法一：使用排序算法，时间复杂度为O(N*lgN)。或者使用选择排序或者交换排序只排序前N个，可以将时间复杂度降至O(N*K)</div>
<div>解法二：使用快速排序。快速排序思想：每一步将待排数据分为两组，其中一组都比另一组的任何数都大。可以通过这个思想，将数组S根据快速排序的一步分为Sa和Sb。若Sa数量小于K，则Sa和从Sb中选择的Top K-|Sa|的元素就是要找的。如果Sa的元素数量大于K，则从Sa中返回最大的K个元素。平均的时间复杂度为O(N).注：编程之美上写的平均复杂度为O(n*lgK)是错误的，解释：July博客</div>
<div>解法三：寻找TOP-K最大的数，构建具有K个元素的最小堆存储这最大的K个数。遍历这N个数，和最小堆存储的TOP-k个数比较（只需要比较根节点）。<strong>未完成：代码实现</strong></div>
<div>
<div>解法四：使用计数排序的思想。加上N个数都是正整数且范围不大，假设为[0,MAX)。则创建一个数组count[MAX]。遍历数组统计出每个数出现的次数（count[i]表示i出现的次数）。这样遍历数组count就能找到TOP-K的数了。<span style="color: #e30000;">若不能保证所有元素都是正整数，且取值范围不大。我们仍可以使用这种思想</span><span style="color: #ad0000;">：</span>将N个数最大的书max和最小的数min分为M块，扫描所有元素统计每个区域的个数，就可以知道第K大的元素在哪个区间，然后在对这个小区间分块处理。</div>
<div></div>
<div>

<hr />

</div>
<div><strong>基数排序：</strong></div>
<div>遍历输入数据A[n]，统计元素i出现的次数c[i]。</div>
<div>对count[i]进行处理，循环c[i]=c[i]+c[i-1]，c数组里面的元素现在代表的是小于或等于i的数量。</div>
<div>再一次遍历A[n]，B[c language="[A[n"][/c]]]=A][n]，同时c[A[n]]--;</div>
</div>
<div>

<hr />

<strong>2.6 精确表达浮点数</strong></div>
<div><span style="color: #939600;">对于一个有限小数如0.34或者一个无限小数如3.3333....即0.3(3)，如何用一个分数精确的表达。</span></div>
<div>当我们获得了一个分数表示小数，这个分数可能需要约分，约分的方法是将A/B简化为(A/Gcd(A,B))/(B/Gcd(A,B))。其中Gcd为求A和B的最大公约数，具体方法见2.7下一节。</div>
<div>如何将一个小数转化为分数呢，一般一个数可能包括整数和小数，我们这里不考虑整数，因为整数可以在用分数表示小数以后直接乘以分母加上分子即可。</div>
<div>现在我们只考虑有限小数0.a1a2a3...an和无限小数0.a1a2..an(b1b2..bm)</div>
<div>对于有限小数，没有啥好说的，直接转为0.a1a2a3...an/(10^n) ——<strong>公式一</strong>。</div>
<div>对于一个无限小数X=0.a1a2a3..an(b1b2..bm)，可以写为X=0.a1a2...an+0.(b1b2..bm)*(10^-n)；令Y=0.(b1b2..bm),则(10^m)Y=b1b2..bm+Y,则Y=b1b2..bm/(10^m -1)；所以X=a1a2..an/(10^n)+(b1b2..bn/(10^m -1))(10^n)=(a1a2..an*(10^m -1)+b1b2..bm)/((10^n)(10^m -1)) ——<strong>公式二</strong></div>
<div>对于所有的小数，都可以带入这两个公式，然后约分即可。</div>
<div>

<hr />

<strong>2.7 最大公约数问题</strong></div>
<div>解法一：辗转相除法：Gcd(x,y)=Gcd(y,x%y).原理：最大公约数g可以整除x和y，x=g*a,y=g*b.x%y=x-(g*b*c)</div>
<div>解法二：辗转相减法:Gcd(x,y)=Gcd(x,x-y)</div>
<div>解法三：分析公约数特点：1.如果y=k*y1,x=k*x1，则f(x,y)=k*f(x1,y1)  2.如果x=p*x1，且p为素数，并且y%p!=0。则f(x,y)=f(x1,y);</div>
<div>使用素数2，若x，y均为偶数，f(x,y)=2*f(x/2,y/2).</div>
<div><span style="font-family: Helvetica, Arial, 'Droid Sans', sans-serif;">若x为偶数，y为</span>奇数，f(x,y)=f(x/2,y)=f(x&gt;&gt;1,y).      同理y为偶数，x为奇数。</div>
<div>若x,y都为奇数，f(x,y)=f(x-y,y)</div>
<div>时间复杂度为O(lg(max(x,y)))</div>
<div>

<hr />

<strong>2.8 寻找符合条件的整数</strong></div>
<div><span style="color: #939600;">给定一个正整数N，求一个最小的正整数M(M&gt;1)，使得N*M的十进制表示形式里面只含有1或者0.</span></div>
<div>直接解法：从2开始枚举M的取值，直至找到符合条件的M。这个方法耗费极大的时间，完全不靠谱。</div>
<div>因为题目是对N*M进行的限定，因此我们可以遍历N*M。将上面的问题转化为：<span style="color: #e30000;">求最小的X，X只含有0或者1，而且X可以被N整除。X/N就是我们要找的M。</span></div>
<div>这样的话，我们只要遍历X的取值：1，10，11，100，101，110，111......如果我们最后得到的X为k位，时间复杂度为O(2^k)</div>
<div>有没有更好的方法？我们发现以下规律：当计算X=10^k+Y是否可以被N整除时，其时Y能否被N整除已经计算过了，那如果我们使用这个信息，是否可以减少操作。</div>
<div>改进的方法思想：</div>
<div>1.记录Y(k位)除以N的余数，当我们要计算10^k+Y是否可以被N整除时，只需要计算10^k%N+Y%N除以N的余数是否为0即可。（如果仅使用这个思想，那么时间复杂度仍然是O(2^k)）</div>
<div>2.加入Y1和Y2的除以N的余数相同，那么10^k+Y1和10^k+Y2的被N整除的余数也相同。那么，将K位的所有Y分为最多N-1个子空间（可能出现的余数），对于每个字空间可能有多个Y，我们只计算最小的Y即可。其他的余数都相同。</div>
<div>算法实现：<a href="https://github.com/shinepengwei/Gallery/blob/master/findANum.cpp">https://github.com/shinepengwei/Gallery/blob/master/findANum.cpp</a></div>
<div><span style="color: #328712;">对于任意的N，一定存在M使得N*M的乘积的十进制表示只有0和1么？感觉应该木有吧- -没想出来证明方法。</span></div>
<div>

<hr />

</div>
<div>2.9</div>
<div></div>
<div></div>
<div></div>
<div>

<hr />

</div>
<div><strong>2.10寻找数组中的最大值和最小值</strong></div>
<div><span style="color: #939600;">对于一个数组，求出他的最大值和最小值需要多少次比较</span></div>
<div>对于这个问题，可以分别求最大值和最小值，需要2n次比较。</div>
<div>如果先对数组处理，将2*i和2*i+1位置上的数比较，令2*i数大于2*i+1的数，这样寻找最大的数只需要在2*i的位置上找，最小的数只需要在2*i+1位置上找，比较次数缩减为0.5n+0.5n+0.5n=1.5n</div>
<div>使用这个方法会破坏原数组，为了不破坏原数组，可以对于每一次2*i和2*i+1位置数的比较结果直接和最大值、最小值比较，就可以不破坏原数组，比较次数也可以为1.5n。</div>
<div>思考：首先对数组进行分了两类，第一类的每个数都比另一类的其中一个大，因此最大数一定在第一类。以此类推，是否可以在对其中一类分成两类，只查找其中一类呢？比较次数为0.5n+0.25n+0.25n（每一类中再分两类）+0.25n+0.25n=1.5n（四个分组，只需要比较其中两个分组，一个分组中找最大值，一个分组中找最小值）。为什么第一次分组减少了0.5n的比较次数，第二次就没有没有减少。原因是第一次分组以后，我们只需要对其中一组进行一个操作（找最大值、找最小值），所以再次分组没有用了。</div>
<div>如果使用分治法，将数组分成两组，每组里面找到最大值和最小值，然后分别比较两个最大值和最小值即可。f(n)=2*f(n/2)+2  =&gt;  f(n)=1.5n-2 比较次数没有减少。</div>
<div>分治法为何没有用？或者说分治法什么时候有用，什么时候没用？需要思考！。</div>
<div>拓展问题：如果需要找到N个数组第二大数，需要比较多少次？是否可以使用类似的分治法思想降低比较次数。</div>
<div>

<hr />

</div>
<div><strong>2.11 寻找最近点对</strong></div>
<div><span style="color: #939600;">给定平面上N个点的坐标，找出距离最近的两个点。</span></div>
<div>解法一：计算两两的距离，额，时间复杂度为O(n^2)</div>
<div>解法二：将N个点根据x坐标排序，然后使用分治法，将N个点根据x坐标M分为Left和Right集合，分别计算他们里面的最近点对，最短距离假设为Mindis(Left)和Mindis(Right)，同时，最近的点对可能分别出现在Left和Right集合。那么，我们要计算横坐标属于[M-mindis,M+mindis]的点之间的距离。对于任意一个区域内的点，我们只要计算按Y坐标轴排列的7个紧挨着它的点的距离即可。（证明过程略）。</div>
<div>所以，时间复杂度为F(N)=2*F(N/2)+O(N).所以他的时间复杂度为O(nlgN)</div>
<div>

<hr />

<strong>2.11 拓展问题：</strong><span style="color: #939600;">给定一个数组arr[0,...N-1]，要求求出相邻两个数的最大差值。相邻指数值相邻，即X和Y，如果不存在数组的其他元素属于[X,Y],则X、Y相邻。</span></div>
<div>编程之美指出可以用求最近点对的思想来做这个，即分治。</div>
<div>我觉得也可以直接对数组排序，然后O(n)时间去找到最大差值，时间复杂度都为O(N*lgN)</div>
<div>更好的方法：抽屉原理：则个数组的最大差值一定不小于(arr.max-arr.min)/N.证明略，反证法，比较好证。</div>
<div>将[arr.max,arr.min]分为N个桶，arr的每个元素都在某一个桶里面，每个桶都有一个最大元素和一个最小元素（可能相同，也可能没有）。O(N)</div>
<div>最大差值一定就是其中一个桶的最大值和下一个桶（存在最小值）的最小值.O(N)</div>
<div>最终，时间复杂度为O(N)，空间复杂度为O(N)。</div>
<div>

<hr />

<strong>2.12快速找到满足条件的两个数</strong></div>
<div><span style="color: #939600;">能否快速找出一个数组的两个数字，使其之和等于给定的值。假设一定存在一组符合要求的解。</span></div>
<div>解法一：穷举，任意取出两个数，计算之和是否是给定的值。</div>
<div>解法二：对于每个数组元素arr[i]，都查找sum-arr[i]是否在数组中，如果直接遍历查找，时间复杂度为O(N^2)，和穷举原理相同。查找一个数组可以通过两种方法加快查找速度：1.先对数组排序，然后O(lgN)时间使用二分法查找。2.使用hash表只需要O(1)时间即可查找。使用这两种方法加快查找时间，总共的时间复杂度分别为O(N*lgN)和O(N)。使用方法2，需要增加O(N)的存储空间。</div>
<div>解法三：在解法二中，我们使用了先排序O(N*lgN)，在对每个arr[i]二分查找Sum-arr[i]，时间复杂度为O(N*lgN)+O(N*lgN)。对于第二步，可以有更好的更好的方法。排序后，数组的第一个元素最小，最后一个元素最大。可以使用两个标识P1，P2分别从数组头和数组尾向中间遍历。如果两个标识指向的元素之和大于SUM，P2向前走一步。如果元素之和小于SUM，P1向后走一步。  这样就可以将查找过程时间复杂度减少到O(N)</div>
<div><span style="color: #ad0000;">经验</span>：给定一个数组，返回两个下标（比如查找两个元素或者查找一个子数组），可以考虑先排序，然后使用两个变量反向遍历。同时，遍历的方向是不变的，从而保证了便利算法的时间复杂度为O(N)。</div>
<div>拓展问题：</div>
<div>1.将两个数字改为三个数字，或者任意数字，怎么办。</div>
<div>三个的话可以先把arr[i]拿出来，然后再在剩下的里面按照找两个数的方法查找SUM-arr[i]，时间复杂度为O(N)*O(N*lgN)。<span style="color: #328712;">任一个数字？背包问题？没想出来。</span></div>
<div>如果完全相等的一对数字找不到，能否找出最接近的解？在使用两个标识反向遍历的时候，每一个计算出的SUM(P1,P2)比较和以前的SUM(P1,P2)与Sum差的绝对值，发现更接近的就记录下来，当两个标识相同时，结束，记录下来的就是最接近的解。当然，也可以吧两个标识存下来。</div>
<div>

<hr />

<strong>2.13 子数组的最大乘积</strong></div>
<div><span style="color: #939600;">给定一个长度为N的整数数组，只允许用乘法，不允许用除法，计算任意（N-1）个数的组合中乘积最大的一组。</span></div>
<div>解法一：找出可能的所有N-1的组合，然后计算结果并比较，找到最大的。时间复杂度为P(N^2)。</div>
<div>解法二：时间换空间。N-1个元素的子数组的乘积p[i]可表示为s[i-1]*t[i+1]。即前I-1个元素的成绩乘上从i+1到最后的元素的乘积。而计算s[i]和t[i]只需要从头到尾和从尾到头遍历一遍，时间复杂度为O(n)，空间复杂度为o（N）.</div>
<div>解法三：直接根据N个数字相乘的结果来获得N-1个数的组合中成绩最大的。</div>
<div>假设P表示N个元素的乘积：</div>
<div>如果P=0，则数组肯定包括0，假设去掉一个0以后，其他N-1个元素的成绩为Q。如果Q为0，则数组中存在两个0，n-1的数的成绩只能为0；如果Q&gt;0,则返回Q；如果Q&lt;0，则选择任意一个负数去掉，N-1个数的最大值也只能为0.</div>
<div>如果P&gt;0，则选择一个绝对值最小的正数去掉。如果没有正数，那就去掉绝对值最大的负数。</div>
<div>如果P&lt;0，则根据负负得正原理，去掉一个绝对值最小的负数。</div>
<div>这样，我们只需要O(n)时间计算数组乘积，然后在用O(n)时间去掉某个数。</div>
<div>解法四：使用解法三可能会导致溢出，因此可以只保存正数、负数和0的个数。</div>
<div>

<hr />

</div>
<div><strong>2.14 数组的子数组之和的最大值</strong></div>
<div><span style="color: #939600;">一个数组，求子数组（连续）的最大值。</span></div>
<div>解法一：穷举法，O(n^3)或者优化到o（N^2）</div>
<div>解法二：分治法。将A[0...n-1]分为A[0,...n/2-1]和A[n/2,n]，分别求出这两段数组各自的最大子数组和，则原数组的最大子数组之和有三种情况：1.与前一数组相同。2.与后一数组相同。3.跨越了两个数组。对于第三种情况，可以遍历两个数组，计算以A[n/2-1]结尾的最大子数组之和、以A[n/2]开始的最大子数组之和，这一步时间复杂度为O(N)。最后找到三种情况的最大值即可。所以时间复杂度F(N)=2F(N/2)+O(N),所以F(N)=O(N*lgN)</div>
<div>解法三：使用动态规划思想，当已经知道了a[0,...i]的最大子数组之和，现在如何计算a[0,...i+1]的最大子数组之和呢？如果以a[i+1]结尾的子数组之和大于a[0..i]的最大子数组之和，则更新。如何计算以a[i+1]结尾的子数组之和呢？如果使用O(n)时间遍历肯定是可以的，但是有没有更好的方法？我们可以保存a[0,..i]以a[i]结尾的最大子数组之和，当a[i+1]出现时，如果a[i]结尾的最大子数组之和大于0，则a[i+1]结尾的最大子数组之和肯定为a[i]结尾的最大子数组之和加上a[i+1]，否则，a[i+1]是以a[i+1]结尾的最大子数组之和。然后比较更新这个a[i+1]结尾的最大子数组之和和a[0,...i]中最大的子数组之和。</div>
<div>算法结构：从头朝后遍历，记录当前0到i的子数组之和的最大值MaxAll以及以a[i]结尾的最大子数组之和MaxLast。对于每个i，如果对于i-1的Maxlast大于0，则MaxLast=MaxtLast+a[i]，否则maxLast=a[i]。然后比较Maxlast和MaxAll，若MaxLast&gt;MaxALL，则更新MaxAll。</div>
<div>伪代码如下：</div>
<div>int maxAll=a[0];</div>
<div>int maxLast=a[0];</div>
<div>for(int i=0;i&lt;n;i++){</div>
<div><span style="font-family: Helvetica, Arial, 'Droid Sans', sans-serif;">     if maxLast&gt;0 :maxLast+=maxLst;</span></div>
<div><span style="font-family: Helvetica, Arial, 'Droid Sans', sans-serif;">     else : maxLast=a[i];</span></div>
<div><span style="font-family: Helvetica, Arial, 'Droid Sans', sans-serif;">     if maxLast&gt;maxAll : maxAll=maxLast;</span></div>
<div>}</div>
<div>return maxAll;</div>
<div>注：解法3和编程之美中的思路是相同的，只不过它的是从后向前，我的从前向后，我认为我的思路更清晰易懂。</div>
<div>

<hr />

</div>
<div><strong>2.15 子数组之和的最大值(二维)</strong></div>
<div><span style="color: #939600;">和2.14不同在于，这里是一个二维数组，希望找一个子二维数组使其元素之和最大。（矩形区域）</span></div>
<div>这个和一维相似，只不过变成了二维。类比一维，可以使用枚举法，在枚举法中也可以对算法进行优化。</div>
<div>我觉得比较好的方法是将二维转化为一维，然后类比一维的方法。先选定Y坐标y1到y2(选择一个小的作为Y轴,min（N,M）)，然后把A[i][y1...y2]的所有元素相加，获得了一维数组，然后使用一维的方法。时间复杂度为O(N*M*min(N,M)).</div>
<div>

<hr />

</div>
<div><strong>2.16求数组中最长的递增子序列</strong></div>
<div><span style="color: #939600;">求一个一维数组中最长的递增子序列的长度，子序列不需要相邻。</span></div>
<div>解法一：使用动态规划。假设目标数组的第i个元素参与到的最长递增子序列（即a[i]为这个最长递增子序列的最后一位）为LIS(i).</div>
<div>我们已经知道了前i个LIS(i)，要计算LIS(i+1),LIS(i+1)=max{1,Lis(k)+1}，其中a[i+1]&gt;a[k]，且k&lt;i。</div>
<div>时间复杂度为O(n^2)</div>
<div>解法二：</div>
<div>使用解法一算法，假设输入为1，-1，2，-3，4，-5，6，-7，我们考虑执行过程。我们发现，LIS[i+1]只要在LIS[0,i]中找到最大的LIS[k]，且a[k]&lt;a[i+1]，则LIS[i+1]=LIS[k]+1;不需要考虑比LIS[k]小的LIS了。</div>
<div>因此，我们可以通过这个对这个算法进行优化。使用MaxV[s]记录长度为s的最长子序列的最后一个元素（最大值）的最小值，如果a[i+1]大于这个最小值，则LIS[i]&gt;=s+1。</div>
<div>综上所述，每次计算LIS[i+1]时，从目前最长的最长子序列s=MaxL开始，用a[i+1]和MaxV[s]比较，如果假设a[i+1]&gt;MaxV[s]，则Lis[i+1]=s+1，同时更新MaxV[s+1];否则，s--;</div>
<div>这个解法时间复杂度编程之美上说的是O（N^2），更确切的应该是O(N*MaxL)。但是MaxL最大可能是n，而且时间复杂度根据输入，所以表面是O(N^2)，其实一般MaxL&lt;N，这个算法是比解法一好的。</div>
<div>解法三：在解法二中在搜索MaxV[s]时是遍历法，所以要O(MaxL)。如果使用二分法呢，就是O(lgMax).这样就将时间复杂度变成了O(N*lgN)</div>
<div>

<hr />

</div>
<div><strong>2.17 数组循环移位</strong></div>
<div><span style="color: #939600;">将含有N个元素的数组右移K位。</span></div>
<div>解法一：每次右移一位，循环K次。时间复杂度为O(N*K)</div>
<div>解法二：我们要考虑全面，K是否可能大于N。若K大于N，我们可以只循环K%N次，这样时间复杂度为O(N^2)。</div>
<div>解法三：三步交换法。如将abcd1234右移3位，结果为234abcd1.第一步：逆序排列abcd1:1dcba234。第二步：逆序排列234：1dcba432.第三步全部逆序：234abcd1</div>
<div>

<hr />

<strong>2.18 数组分割</strong></div>
<div><span style="color: #939600;">将元素个数为2N的数组，分割为两个长度为N的数组（不要求在原数组连续），并且两数组之和最接近？</span></div>
<div>编程之美中给的结果只求除了两数组之和是多少，并没有求出如何分割元素。</div>
<div>解法如下：将任务分为2N步，第k步求出前k个元素中任意i个元素之和。在第k+1步中，根据前k步的结果和a[k]更新任意i个元素之和。不需要计算超过n的i。</div>
<div></div>
<div>

<hr />

</div>
<div><strong>2.19 区间重合判断</strong></div>
<div><span style="color: #939600;">给定原区间[x,y](y&gt;=x)和N个无序目标区间[x1,y1],[x2,y2],....[xn,yn],判断原区间是否包含在目标区间。</span></div>
<div>解法一：我首先想到的<a>是将目标区间处理成一</a>个或多个不相交的大区间，然后去判断原区间是否在这几个大区间之内。步骤：1.对目标区间根据x排序。2.将目标区间合并为M个大区间。3.<a>查找原区间是否包含在某一</a>个大区间内，可以使用二分法查找。时间复杂度为O(N*lgN)+O(N)+O(lgN)</div>
<div>解法二：（编程之美上的解法一）因为解法一可能需要将目标区间处理为多个不相交的大区间，不优雅。而编程之美上的一个方法是将目标区间投影到原区间上，看最后是否将所有的原区间覆盖。编程之美上说这个方法的时间复杂度为O(N^2)，我没看懂。。。</div>
<div>解法三：<span style="color: #6a0081;">我的想法：</span>可以先对目标数组根据X排序，然后去对原区间压缩，而不会造成切割，最后去判断是否原区间被全部压缩掉。时间复杂度：O(N*lgN)+O(N)，也没有额外的空间，这个应该是更好的方法。</div>
<div>扩展问题：如何处理二维空间覆盖问题？</div>
<div>判断一个原矩形在二维空间是否被其他多个目标矩形覆盖，可以对X轴和Y轴分别进行一维区间重合判断。（有没有更好的方法？路上思考下）</div>
<div></div>
<div></div>
<div><strong>3.9 重建二叉树</strong></div>
<div><span style="color: #939600;">根据前序遍历和中序遍历重建二叉树。</span></div>
<div>前序遍历的第一个节点是根节点，然后找到中须遍历的这个节点，中须遍历数组中这个节点的前面的序列就是根节点的左子树，后面的节点序列就是根节点的右子树，递归。</div>
<div>     再注：看到了拓展问题2，明白了这些判断的意义和价值。如果前序遍历和中须遍历给的不合理，这些判断可以判断出来。</div>
<div>扩展问题：</div>
<div>1.如果根据字母不能确定节点，即有些节点的字母可能相同的。该题会出现什么情况？</div>
<div>答：重构出来的二叉树肯定不唯一。可以将所有的可能的情况都找出来。思想相同，更复杂些。</div>
<div>2.如何判断给定的前序遍历和中序遍历的结果是合理的？</div>
<div>能否根据算法构造出一个对应的二叉树。如前序：acb，中序bac，就没有办法构造出二叉树。</div>
<div>3.如果知道前序和后序遍历的结果，能重构二叉树么？</div>
<div>不能！当一个节点只有左子树或右子树（一个子树）时，无法分辨出来是左还是右子树。</div>
<div></div>
<div></div>
<div>

<hr />

</div>
<div>二叉树的遍历</div>
<div>前序遍历：</div>
<div>PreTraverse(Node *p){</div>
<div>     if(p==NULL) return;</div>
<div>      printf(p-&gt;value);</div>
<div>     if(p-&gt;left!=NULL){</div>
<div>          PreTraverse(p-&gt;left);</div>
<div>     }</div>
<div>     if(p-&gt;right!=NULL){</div>
<div>          PreTraverse(p-&gt;right)</div>
<div>     }</div>
<div>}</div>
<div>中序遍历：</div>
<div>MidTraverse(Node *p){</div>
<div>     if(p==NULL) return;</div>
<div>     if(p-&gt;left!=NULL){</div>
<div>         MidTraverse(p-&gt;left);</div>
<div>     }</div>
<div>      printf(p-&gt;value);</div>
<div>     if(p-&gt;right!=NULL){</div>
<div>         MidTraverse(p-&gt;right)</div>
<div>     }</div>
<div>}</div>
<div>中序遍历不使用递归版本一（只用栈存储遍历状态）</div>
<div>MidTraverse(Node *p){</div>
<div>     if(p==NULL) return;</div>
<div>     stack&lt;Node *&gt; Nstack;</div>
<div>     Nstack.push(p);</div>
<div>     while(!Nstack.isEmpty()){</div>
<div>         tmp_Ptr=Nstack.getTop();</div>
<div>          //如果tmp_Ptr==NULL，说明上一次访问的是一个右子树为NULL的节点</div>
<div>          while(tmp_Ptr!=NULL) Nstack.push(tmp_Ptr-&gt;left);tmp_Ptr=tmp_Ptr-&gt;left;//向左走到尽头最后会将一个NULL指针放在stack的顶端</div>
<div>          Nstack.pop();//将NULL指针去掉</div>
<div>          if(!Nstack.isEmpty()){</div>
<div>               tmp_Ptr=Nstack.pop();</div>
<div>               printf(tmp_Ptr-&gt;value);</div>
<div>               Nstack.push(tmp_Ptr-&gt;right);//如果右指针指向NULL，也将其放入stack的顶端，以标识</div>
<div>          }</div>
<div>     }</div>
<div>}</div>
<div>中序遍历不使用递归版本二，这个更好理解</div>
<div>MidTraverse(Node *T){</div>
<div>     Node *p=T;</div>
<div>     stack&lt;Node*&gt; Nstack;</div>
<div>     while(p||!Nstack.isEmpty()){</div>
<div>          if(p!=Null){</div>
<div>               Nstack.push(p);</div>
<div>               p=p-&gt;left;</div>
<div>          }else{</div>
<div>               p=Nstack.pop();</div>
<div>               printf(p-&gt;value);</div>
<div>               p=p-&gt;right;</div>
<div>          }</div>
<div>     }</div>
<div>}</div>
<div>后序遍历：</div>
<div>AfterTraverse(Node *p){</div>
<div>      if(p==NULL) return;</div>
<div>     if(p-&gt;left!=NULL){</div>
<div>        AfterTraverse(p-&gt;left);</div>
<div>     }</div>
<div>      printf(p-&gt;value);</div>
<div>     if(p-&gt;right!=NULL){</div>
<div>        AfterTraverse(p-&gt;right)</div>
<div>     }</div>
<div>}</div>
<div></div>
<div>

<hr />

<strong>3.10分层遍历二叉树</strong></div>
<div>问题1：给定一颗二叉树，分层遍历该二叉树。</div>
<div>使用队列，输出这个节点的同时将他的左右子节点存入队列，先进先出。同时想法子标识一层的结束用于分行。</div>
<div>问题2：打印二叉树某层次节点（从左到右），根节点为0层。</div>
<div>可使用递归，int printNodeAtLevel(Node *root,int level),若level&gt;0，则递归调用root的左右子节点，并将level-1,当level=<a>0时</a>左右边界条件输出并返回。</div>
<div>拓展问题：如果从下到上访问，咋搞？</div>
<div>使用栈，先访问右节点，在访问左节点，先进后出。打印出来即可。</div>
<div>

<hr />

</div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
&nbsp;

<hr />

对 2013年9月5日 1:40:21 的修改出现冲突：

<hr />

<div><strong>2.1求二进制数中1的个数</strong></div>
<div>解法一：将数除以2至0，余数为1的次数数</div>
<div>解法二：使用位运算：将该数与0x01与运算，然后左移1位。与运算结果为1的次数。位操作和除、余操作相比效率更高。</div>
<div>解法三：让算法的复杂度只和1有关，原理是v&amp;(v-1)后的结果即为将v二进制数的为1的最低位消除。</div>
<div>解法四：使用分治判断法判断一个数的1的位数，但是根据程序的书写最多可能要判断255此才可以，因此此方法不太好。</div>
<div>解法五：使用分治判断不好，那可以使用hash将二进制数(key)映射到1的个数(value)，只要前期将hash表建好即可。这种方法是使用空间换时间，若二进制位数较少如八位，这个是很好的算法，但是若位数很多如32位，这个方法就不好了。</div>
<div>拓展问题：两个二进制整数A、B，他们有多少位是不同的？</div>
<div>答：异或操作，结果然后计算1的个数。</div>
<div>

<hr />

<strong>2.2 不要被阶乘吓到</strong></div>
<div></div>
<div></div>
<div>

<hr />

</div>
<div><strong>2.3寻找水王</strong></div>
<div><span style="color: #939600;">一个ID发的帖子超过了所有帖子总数量的一般，如何找到这个ID。</span></div>
<div>解法一：对所有帖子的ID排序，然后找到中间的那一个ID即可。需要排序，时间复杂度：O(N*lgN)</div>
<div>解法二：每次删掉两个不同的ID，最后剩下的就是要找的ID。只需要遍历一遍，O(N)即可。</div>
<div><strong>代码也很巧妙</strong>，遍历ID列表，找到一个ID作为candidate，设置一个time来保存这个ID出现的次数。继续遍历，如果新的ID和这个ID相同，time加以。如果不同，time-1（这个行为就是删掉两个不同的ID）。如果time为0，选择新的candidate。</div>
<div>扩展问题：有三个发帖很多的ID，每个人的发帖数目超过了全部的1/4，如何快速找到这3个ID？</div>
<div>同理，每次删掉四个不同的ID，最后剩下3个ID即可。</div>
<div>很多问题都可以用这种思想解决：考虑如何把一个问题转换成规模较小的若干问题。分治、递推和贪心都是这样的思路。转化过程中，小的问题跟原问题本质上一致。如这个问题，我们保证了问题的解在小问题中仍然具有和原问题相同的性质：水王的ID在ID列表中的数量超过一半。</div>
<div>

<hr />

<strong>2.4  1的数目</strong></div>
<div></div>
<div><strong>2.5 寻找最大的K个数</strong></div>
<div>解法一：使用排序算法，时间复杂度为O(N*lgN)。或者使用选择排序或者交换排序只排序前N个，可以将事件负责度降至O(N*K)</div>
<div>解法二：使用快速排序。快速排序思想：每一步将待排数据分为两组，其中一组都比另一组的任何数都大。可以通过这个思想，将数组S根据快速排序的一步分为Sa和Sb。若Sa数量小于K，则Sa和从Sb中选择的Top K-|Sa|的元素就是要找的。如果Sa的元素数量大于K，则从Sa中返回最大的K个元素。平均的时间复杂度为O(N).注：编程之美上写的平均复杂度为O(n*lgK)是错误的，解释：July博客</div>
<div>解法三：寻找TOP-K最大的数，构建具有K个元素的最小堆存储这最大的K个数。遍历这N个数，和最小堆存储的TOP-k个数比较（只需要比较根节点）。<strong>未完成：代码实现</strong></div>
<div>
<div>解法四：使用计数排序的思想。加上N个数都是正整数且范围不大，假设为[0,MAX)。则创建一个数组count[MAX]。遍历数组统计出每个数出现的次数（count[i]表示i出现的次数）。这样遍历数组count就能找到TOP-K的数了。<span style="color: #e30000;">若不能保证所有元素都是正整数，且取值范围不大。我们仍可以使用这种思想</span><span style="color: #ad0000;">：</span>将N个数最大的书max和最小的数min分为M块，扫描所有元素统计每个区域的个数，就可以知道第K大的元素在哪个区间，然后在对这个小区间分块处理。</div>
<div></div>
<div>

<hr />

</div>
<div><strong>基数排序：</strong></div>
<div>遍历输入数据A[n]，统计元素i出现的次数c[i]。</div>
<div>对count[i]进行处理，循环c[i]=c[i]+c[i-1]，c数组里面的元素现在代表的是小于或等于i的数量。</div>
<div>再一次遍历A[n]，B[c language="[A[n"][/c]]]=A][n]，同时c[A[n]]--;</div>
</div>
<div>

<hr />

<strong>2.6 精确表达浮点数</strong></div>
<div><span style="color: #939600;">对于一个有限小数如0.34或者一个无限小数如3.3333....即0.3(3)，如何用一个分数精确的表达。</span></div>
<div>当我们获得了一个分数表示小数，这个分数可能需要约分，约分的方法是将A/B简化为(A/Gcd(A,B))/(B/Gcd(A,B))。其中Gcd为求A和B的最大公约数，具体方法见2.7下一节。</div>
<div>如何将一个小数转化为分数呢，一般一个数可能包括整数和小数，我们这里不考虑整数，因为整数可以在用分数表示小数以后直接乘以分母加上分子即可。</div>
<div>现在我们只考虑有限小数0.a1a2a3...an和无限小数0.a1a2..an(b1b2..bm)</div>
<div>对于有限小数，没有啥好说的，直接转为0.a1a2a3...an/(10^n) ——<strong>公式一</strong>。</div>
<div>对于一个无限小数X=0.a1a2a3..an(b1b2..bm)，可以写为X=0.a1a2...an+0.(b1b2..bm)*(10^-n)；令Y=0.(b1b2..bm),则(10^m)Y=b1b2..bm+Y,则Y=b1b2..bm/(10^m -1)；所以X=a1a2..an/(10^n)+(b1b2..bn/(10^m -1))(10^n)=(a1a2..an*(10^m -1)+b1b2..bm)/((10^n)(10^m -1)) ——<strong>公式二</strong></div>
<div>对于所有的小数，都可以带入这两个公式，然后约分即可。</div>
<div>

<hr />

<strong>2.7 最大公约数问题</strong></div>
<div>解法一：辗转相除法：Gcd(x,y)=Gcd(y,x%y).原理：最大公约数g可以整除x和y，x=g*a,y=g*b.x%y=x-(g*b*c)</div>
<div>解法二：辗转相减法:Gcd(x,y)=Gcd(x,x-y)</div>
<div>解法三：分析公约数特点：1.如果y=k*y1,x=k*x1，则f(x,y)=k*f(x1,y1)  2.如果x=p*x1，且p为素数，并且y%p!=0。则f(x,y)=f(x1,y);</div>
<div>使用素数2，若x，y均为偶数，f(x,y)=2*f(x/2,y/2).</div>
<div><span style="font-family: Helvetica, Arial, 'Droid Sans', sans-serif;">若x为偶数，y为</span>奇数，f(x,y)=f(x/2,y)=f(x&gt;&gt;1,y).      同理y为偶数，x为奇数。</div>
<div>若x,y都为奇数，f(x,y)=f(x-y,y)</div>
<div>时间复杂度为O(lg(max(x,y)))</div>
<div>

<hr />

<strong>2.8 寻找符合条件的整数</strong></div>
<div><span style="color: #939600;">给定一个正整数N，求一个最小的正整数M(M&gt;1)，使得N*M的十进制表示形式里面只含有1或者0.</span></div>
<div>直接解法：从2开始枚举M的取值，直至找到符合条件的M。这个方法耗费极大的时间，完全不靠谱。</div>
<div>因为题目是对N*M进行的限定，因此我们可以遍历N*M。将上面的问题转化为：<span style="color: #e30000;">求最小的X，X只含有0或者1，而且X可以被N整除。X/N就是我们要找的M。</span></div>
<div>这样的话，我们只要遍历X的取值：1，10，11，100，101，110，111......如果我们最后得到的X为k位，时间复杂度为O(2^k)</div>
<div>有没有更好的方法？我们发现以下规律：当计算X=10^k+Y是否可以被N整除时，其时Y能否被N整除已经计算过了，那如果我们使用这个信息，是否可以减少操作。</div>
<div>改进的方法思想：</div>
<div>1.记录Y(k位)除以N的余数，当我们要计算10^k+Y是否可以被N整除时，只需要计算10^k%N+Y%N除以N的余数是否为0即可。（如果仅使用这个思想，那么时间复杂度仍然是O(2^k)）</div>
<div>2.加入Y1和Y2的除以N的余数相同，那么10^k+Y1和10^k+Y2的被N整除的余数也相同。那么，将K位的所有Y分为最多N-1个子空间（可能出现的余数），对于每个字空间可能有多个Y，我们只计算最小的Y即可。其他的余数都相同。</div>
<div>算法实现：https://github.com/shinepengwei/Gallery/blob/master/findANum.cpp</div>
<div><span style="color: #328712;">对于任意的N，一定存在M使得N*M的乘积的十进制表示只有0和1么？感觉应该木有吧- -没想出来证明方法。</span></div>
<div>

<hr />

</div>
<div>2.9</div>
<div></div>
<div></div>
<div></div>
<div>

<hr />

</div>
<div><strong>2.10寻找数组中的最大值和最小值</strong></div>
<div><span style="color: #939600;">对于一个数组，求出他的最大值和最小值需要多少次比较</span></div>
<div>对于这个问题，可以分别求最大值和最小值，需要2n次比较。</div>
<div>如果先对数组处理，将2*i和2*i+1位置上的数比较，令2*i数大于2*i+1的数，这样寻找最大的数只需要在2*i的位置上找，最小的数只需要在2*i+1位置上找，比较次数缩减为0.5n+0.5n+0.5n=1.5n</div>
<div>使用这个方法会破坏原数组，为了不破坏原数组，可以对于每一次2*i和2*i+1位置数的比较结果直接和最大值、最小值比较，就可以不破坏原数组，比较次数也可以为1.5n。</div>
<div>思考：首先对数组进行分了两类，第一类的每个数都比另一类的其中一个大，因此最大数一定在第一类。以此类推，是否可以在对其中一类分成两类，只查找其中一类呢？比较次数为0.5n+0.25n+0.25n（每一类中再分两类）+0.25n+0.25n=1.5n（四个分组，只需要比较其中两个分组，一个分组中找最大值，一个分组中找最小值）。为什么第一次分组减少了0.5n的比较次数，第二次就没有没有减少。原因是第一次分组以后，我们只需要对其中一组进行一个操作（找最大值、找最小值），所以再次分组没有用了。</div>
<div>如果使用分治法，将数组分成两组，每组里面找到最大值和最小值，然后分别比较两个最大值和最小值即可。f(n)=2*f(n/2)+2  =&gt;  f(n)=1.5n-2 比较次数没有减少。</div>
<div>分治法为何没有用？或者说分治法什么时候有用，什么时候没用？需要思考！。</div>
<div>拓展问题：如果需要找到N个数组第二大数，需要比较多少次？是否可以使用类似的分治法思想降低比较次数。</div>
<div>

<hr />

</div>
<div><strong>2.11 寻找最近点对</strong></div>
<div><span style="color: #939600;">给定平面上N个点的坐标，找出距离最近的两个点。</span></div>
<div>解法一：计算两两的距离，额，时间复杂度为O(n^2)</div>
<div>解法二：将N个点根据x坐标排序，然后使用分治法，将N个点根据x坐标M分为Left和Right集合，分别计算他们里面的最近点对，最短距离假设为Mindis(Left)和Mindis(Right)，同时，最近的点对可能分别出现在Left和Right集合。那么，我们要计算横坐标属于[M-mindis,M+mindis]的点之间的距离。对于任意一个区域内的点，我们只要计算按Y坐标轴排列的7个紧挨着它的点的距离即可。（证明过程略）。</div>
<div>所以，时间复杂度为F(N)=2*F(N/2)+O(N).所以他的时间复杂度为O(nlgN)</div>
<div>

<hr />

<strong>2.11 拓展问题：</strong><span style="color: #939600;">给定一个数组arr[0,...N-1]，要求求出相邻两个数的最大差值。相邻指数值相邻，即X和Y，如果不存在数组的其他元素属于[X,Y],则X、Y相邻。</span></div>
<div>编程之美指出可以用求最近点对的思想来做这个，即分治。</div>
<div>我觉得也可以直接对数组排序，然后O(n)时间去找到最大差值，时间复杂度都为O(N*lgN)</div>
<div>更好的方法：抽屉原理：则个数组的最大差值一定不小于(arr.max-arr.min)/N.证明略，反证法，比较好证。</div>
<div>将[arr.max,arr.min]分为N个桶，arr的每个元素都在某一个桶里面，每个桶都有一个最大元素和一个最小元素（可能相同，也可能没有）。O(N)</div>
<div>最大差值一定就是其中一个桶的最大值和下一个桶（存在最小值）的最小值.O(N)</div>
<div>最终，时间复杂度为O(N)，空间复杂度为O(N)。</div>
<div>

<hr />

<strong>2.12快速找到满足条件的两个数</strong></div>
<div><span style="color: #939600;">能否快速找出一个数组的两个数字，使其之和等于给定的值。假设一定存在一组符合要求的解。</span></div>
<div>解法一：穷举，任意取出两个数，计算之和是否是给定的值。</div>
<div>解法二：对于每个数组元素arr[i]，都查找sum-arr[i]是否在数组中，如果直接遍历查找，时间复杂度为O(N^2)，和穷举原理相同。查找一个数组可以通过两种方法加快查找速度：1.先对数组排序，然后O(lgN)时间使用二分法查找。2.使用hash表只需要O(1)时间即可查找。使用这两种方法加快查找时间，总共的时间复杂度分别为O(N*lgN)和O(N)。使用方法2，需要增加O(N)的存储空间。</div>
<div>解法三：在解法二中，我们使用了先排序O(N*lgN)，在对每个arr[i]二分查找Sum-arr[i]，时间复杂度为O(N*lgN)+O(N*lgN)。对于第二步，可以有更好的更好的方法。排序后，数组的第一个元素最小，最后一个元素最大。可以使用两个标识P1，P2分别从数组头和数组尾向中间遍历。如果两个标识指向的元素之和大于SUM，P2向前走一步。如果元素之和小于SUM，P1向后走一步。  这样就可以将查找过程时间复杂度减少到O(N)</div>
<div><span style="color: #ad0000;">经验</span>：给定一个数组，返回两个下标（比如查找两个元素或者查找一个子数组），可以考虑先排序，然后使用两个变量反向遍历。同时，遍历的方向是不变的，从而保证了便利算法的时间复杂度为O(N)。</div>
<div>拓展问题：</div>
<div>1.将两个数字改为三个数字，或者任意数字，怎么办。</div>
<div>三个的话可以先把arr[i]拿出来，然后再在剩下的里面按照找两个数的方法查找SUM-arr[i]，时间复杂度为O(N)*O(N*lgN)。<span style="color: #328712;">任一个数字？背包问题？没想出来。</span></div>
<div>如果完全相等的一对数字找不到，能否找出最接近的解？在使用两个标识反向遍历的时候，每一个计算出的SUM(P1,P2)比较和以前的SUM(P1,P2)与Sum差的绝对值，发现更接近的就记录下来，当两个标识相同时，结束，记录下来的就是最接近的解。当然，也可以吧两个标识存下来。</div>
<div>

<hr />

<strong>2.13 子数组的最大乘积</strong></div>
<div><span style="color: #939600;">给定一个长度为N的整数数组，只允许用乘法，不允许用除法，计算任意（N-1）个数的组合中乘积最大的一组。</span></div>
<div>解法一：找出可能的所有N-1的组合，然后计算结果并比较，找到最大的。时间复杂度为P(N^2)。</div>
<div>解法二：时间换空间。N-1个元素的子数组的乘积p[i]可表示为s[i-1]*t[i+1]。即前I-1个元素的成绩乘上从i+1到最后的元素的乘积。而计算s[i]和t[i]只需要从头到尾和从尾到头遍历一遍，时间复杂度为O(n)，空间复杂度为o（N）.</div>
<div>解法三：直接根据N个数字相乘的结果来获得N-1个数的组合中成绩最大的。</div>
<div>假设P表示N个元素的乘积：</div>
<div>如果P=0，则数组肯定包括0，假设去掉一个0以后，其他N-1个元素的成绩为Q。如果Q为0，则数组中存在两个0，n-1的数的成绩只能为0；如果Q&gt;0,则返回Q；如果Q&lt;0，则选择任意一个负数去掉，N-1个数的最大值也只能为0.</div>
<div>如果P&gt;0，则选择一个绝对值最小的正数去掉。如果没有正数，那就去掉绝对值最大的负数。</div>
<div>如果P&lt;0，则根据负负得正原理，去掉一个绝对值最小的负数。</div>
<div>这样，我们只需要O(n)时间计算数组乘积，然后在用O(n)时间去掉某个数。</div>
<div>解法四：使用解法三可能会导致溢出，因此可以只保存正数、负数和0的个数。</div>
<div>

<hr />

</div>
<div><strong>2.14 数组的子数组之和的最大值</strong></div>
<div><span style="color: #939600;">一个数组，求子数组（连续）的最大值。</span></div>
<div>解法一：穷举法，O(n^3)或者优化到o（N^2）</div>
<div>解法二：分治法。将A[0...n-1]分为A[0,...n/2-1]和A[n/2,n]，分别求出这两段数组各自的最大子数组和，则原数组的最大子数组之和有三种情况：1.与前一数组相同。2.与后一数组相同。3.跨越了两个数组。对于第三种情况，可以遍历两个数组，计算以A[n/2-1]结尾的最大子数组之和、以A[n/2]开始的最大子数组之和，这一步时间复杂度为O(N)。最后找到三种情况的最大值即可。所以时间复杂度F(N)=2F(N/2)+O(N),所以F(N)=O(N*lgN)</div>
<div>解法三：使用动态规划思想，当已经知道了a[0,...i]的最大子数组之和，现在如何计算a[0,...i+1]的最大子数组之和呢？如果以a[i+1]结尾的子数组之和大于a[0..i]的最大子数组之和，则更新。如何计算以a[i+1]结尾的子数组之和呢？如果使用O(n)时间遍历肯定是可以的，但是有没有更好的方法？我们可以保存a[0,..i]以a[i]结尾的最大子数组之和，当a[i+1]出现时，如果a[i]结尾的最大子数组之和大于0，则a[i+1]结尾的最大子数组之和肯定为a[i]结尾的最大子数组之和加上a[i+1]，否则，a[i+1]是以a[i+1]结尾的最大子数组之和。然后比较更新这个a[i+1]结尾的最大子数组之和和a[0,...i]中最大的子数组之和。</div>
<div>算法结构：从头朝后遍历，记录当前0到i的子数组之和的最大值MaxAll以及以a[i]结尾的最大子数组之和MaxLast。对于每个i，如果对于i-1的Maxlast大于0，则MaxLast=MaxtLast+a[i]，否则maxLast=a[i]。然后比较Maxlast和MaxAll，若MaxLast&gt;MaxALL，则更新MaxAll。</div>
<div>伪代码如下：</div>
<div>int maxAll=a[0];</div>
<div>int maxLast=a[0];</div>
<div>for(int i=0;i&lt;n;i++){</div>
<div><span style="font-family: Helvetica, Arial, 'Droid Sans', sans-serif;">     if maxLast&gt;0 :maxLast+=maxLst;</span></div>
<div><span style="font-family: Helvetica, Arial, 'Droid Sans', sans-serif;">     else : maxLast=a[i];</span></div>
<div><span style="font-family: Helvetica, Arial, 'Droid Sans', sans-serif;">     if maxLast&gt;maxAll : maxAll=maxLast;</span></div>
<div>}</div>
<div>return maxAll;</div>
<div>注：解法3和编程之美中的思路是相同的，只不过它的是从后向前，我的从前向后，我认为我的思路更清晰易懂。</div>
<div>

<hr />

</div>
<div><strong>2.15 子数组之和的最大值(二维)</strong></div>
<div><span style="color: #939600;">和2.14不同在于，这里是一个二维数组，希望找一个子二维数组使其元素之和最大。（矩形区域）</span></div>
<div>这个和一维相似，只不过变成了二维。类比一维，可以使用枚举法，在枚举法中也可以对算法进行优化。</div>
<div>我觉得比较好的方法是将二维转化为一维，然后类比一维的方法。先选定Y坐标y1到y2(选择一个小的作为Y轴,min（N,M）)，然后把A[i][y1...y2]的所有元素相加，获得了一维数组，然后使用一维的方法。时间复杂度为O(N*M*min(N,M)).</div>
<div>

<hr />

</div>
<div><strong>2.16求数组中最长的递增子序列</strong></div>
<div><span style="color: #939600;">求一个一维数组中最长的递增子序列的长度，子序列不需要相邻。</span></div>
<div>解法一：使用动态规划。假设目标数组的第i个元素参与到的最长递增子序列（即a[i]为这个最长递增子序列的最后一位）为LIS(i).</div>
<div>我们已经知道了前i个LIS(i)，要计算LIS(i+1),LIS(i+1)=max{1,Lis(k)+1}，其中a[i+1]&gt;a[k]，且k&lt;i。</div>
<div>时间复杂度为O(n^2)</div>
<div>解法二：</div>
<div>使用解法一算法，假设输入为1，-1，2，-3，4，-5，6，-7，我们考虑执行过程。我们发现，LIS[i+1]只要在LIS[0,i]中找到最大的LIS[k]，且a[k]&lt;a[i+1]，则LIS[i+1]=LIS[k]+1;不需要考虑比LIS[k]小的LIS了。</div>
<div>因此，我们可以通过这个对这个算法进行优化。使用MaxV[s]记录长度为s的最长子序列的最后一个元素（最大值）的最小值，如果a[i+1]大于这个最小值，则LIS[i]&gt;=s+1。</div>
<div>综上所述，每次计算LIS[i+1]时，从目前最长的最长子序列s=MaxL开始，用a[i+1]和MaxV[s]比较，如果假设a[i+1]&gt;MaxV[s]，则Lis[i+1]=s+1，同时更新MaxV[s+1];否则，s--;</div>
<div>这个解法时间复杂度编程之美上说的是O（N^2），更确切的应该是O(N*MaxL)。但是MaxL最大可能是n，而且时间复杂度根据输入，所以表面是O(N^2)，其实一般MaxL&lt;N，这个算法是比解法一好的。</div>
<div>解法三：在解法二中在搜索MaxV[s]时是遍历法，所以要O(MaxL)。如果使用二分法呢，就是O(lgMax).这样就将时间复杂度变成了O(N*lgN)</div>
<div>

<hr />

</div>
<div><strong>2.17 数组循环移位</strong></div>
<div><span style="color: #939600;">将含有N个元素的数组右移K位。</span></div>
<div>解法一：每次右移一位，循环K次。时间复杂度为O(N*K)</div>
<div>解法二：我们要考虑全面，K是否可能大于N。若K大于N，我们可以只循环K%N次，这样时间复杂度为O(N^2)。</div>
<div>解法三：三步交换法。如将abcd1234右移3位，结果为234abcd1.第一步：逆序排列abcd1:1dcba234。第二步：逆序排列234：1dcba432.第三步全部逆序：234abcd1</div>
<div>

<hr />

<strong>2.18 数组分割</strong></div>
<div><span style="color: #939600;">将元素个数为2N的数组，分割为两个长度为N的数组（不要求在原数组连续），并且两数组之和最接近？</span></div>
<div>编程之美中给的结果只求除了两数组之和是多少，并没有求出如何分割元素。</div>
<div>解法如下：将任务分为2N步，第k步求出前k个元素中任意i个元素之和。在第k+1步中，根据前k步的结果和a[k]更新任意i个元素之和。不需要计算超过n的i。</div>
<div></div>
<div>

<hr />

</div>
<div><strong>2.19 区间重合判断</strong></div>
<div><span style="color: #939600;">给定原区间[x,y](y&gt;=x)和N个无序目标区间[x1,y1],[x2,y2],....[xn,yn],判断原区间是否包含在目标区间。</span></div>
<div>解法一：我首先想到的是将目标区间处理成一个或多个不相交的大区间，然后去判断原区间是否在这几个大区间之内。步骤：1.对目标区间根据x排序。2.将目标区间合并为M个大区间。3.查找原区间是否包含在某一个大区间内，可以使用二分法查找。时间复杂度为O(N*lgN)+O(N)+O(lgN)</div>
<div>解法二：（编程之美上的解法一）因为解法一可能需要将目标区间处理为多个不相交的大区间，不优雅。而编程之美上的一个方法是将目标区间投影到原区间上，看最后是否将所有的原区间覆盖。编程之美上说这个方法的时间复杂度为O(N^2)，我没看懂。。。</div>
<div>解法三：<span style="color: #6a0081;">我的想法：</span>可以先对目标数组根据X排序，然后去对原区间压缩，而不会造成切割，最后去判断是否原区间被全部压缩掉。时间复杂度：O(N*lgN)+O(N)，也没有额外的空间，这个应该是更好的方法。</div>
<div>扩展问题：如何处理二维空间覆盖问题？</div>
<div>判断一个原矩形在二维空间是否被其他多个目标矩形覆盖，可以对X轴和Y轴分别进行一维区间重合判断。（有没有更好的方法？路上思考下）</div>
<div>

<hr />

<strong>3.1 字符串移位包含的问题</strong></div>
<div><span style="color: #939600;">给定两个字符串s1和s2，要求判断s2是否能够被s1做循环移位得到的字符串包含。</span></div>
<div>char *strstr(char *s1,char *s2)函数可以判断s2是否包含的在s1中，并返回s1中包含的s2的指针，否则返回NULL。</div>
<div>解法一：对s1移位，每次移位一次然后判断是否包含了s2.效率很低。</div>
<div>解法二：将S1扩展两倍，在s1后面再加上s1，如原本的s1=“abcd”,扩展后的s1=“abcdabcd”，然后判断s2是否包含在扩展后的s1中。</div>
<div>我的想法：其实可以不申请多的空间，只要不使用strstr函数，每次从s1[i]开始，判断s[i]开始的strlen(s2)个字符是否和s2相同，判断期间j=i+x,每次j++,判断s1[j]是否等于s2[x],如果j&gt;=n，令j=j%n;</div>
<div>

<hr />

<strong>3.2电话号码对应英语单词</strong></div>
<div><span style="color: #939600;">电话号码盘一般可以用于输入字母。如2表示a/b/c，3表示d/e/f。对于一个号码，可以依次输出其代表的字母组合。</span></div>
<div>其实这个问题就是对一棵树进行遍历，把每个从根节点到叶子节点的路径都输出来。</div>
<div>可以使用直接循环法，使用answer[]记录每位号码对应的字母，把answer的所有结果输出即可。但是因为电话号码的长度不确定，因此文章中给出了一个更优雅的循环方法。当然也可以使用递归的方法。</div>
<div>这个问题可以扩展为如何输出n位数（M进制）的所有可能的数。</div>
<div>

<hr />

</div>
<div><strong>3.3计算字符串的相似度</strong></div>
<div>对于两个字符串，希望判断其相似度。操作方法：1.修改一个字符。2.增加一个字符。3.删除一个字符。定义两个字符串的距离为一个字符串变成另一个操作的次数，相似度为1/（距离）。</div>
<div>思想是将这个问题转化成规模较小的问题，如果两个字符串A[1..N]和B[1..N]，第一个字符相同，那么只需要计算A[2..N]和B[2...N]的距离。如果第一个字符不相同，那就进行一部操作后，分三种情况，1.计算A[2...N]和B[1...N]的距离（A删掉一个字符，或者B添加一个字符）。2.计算A[1...N]和B[2...N]的距离（A添加一个字符或者B删掉一个字符）。3.计算A[2...N]和B[2...N]的距离（把A和B的当前字符改成相同）。递归就可以写出来一个程序。</div>
<div>在递归过程中，如果因为有些子问题是可能重复的，可以将这些子问题的结果存储下来，下次使用时不用计算直接查询。</div>
<div>

<hr />

<strong>3.4 从无头的单链表中删除节点。</strong></div>
<div>删掉这个节点的下一个节点，然后把下一个节点的内容复制到这个节点。</div>
<div>

<hr />

</div>
<div><strong>3.6 变成判断两个链表是否相交。</strong></div>
<div><span style="color: #939600;">如h1:a1-&gt;...-&gt;b1-&gt;b2和链表H2:c1-&gt;...-&gt;b1-&gt;b2相较于b1.只判断是否相交，假设不带环。</span></div>
<div>解法1：直接判断一个链表的每个节点是否包含在另一个连接。时间复杂度很大。</div>
<div>解法2：使用hash表存储H1链表的节点，判断h2链表中的节点在hash表中是否存在。</div>
<div>解法3：可以将H1的最后的节点指向h2头结点，然后从h2开始向后遍历，看看是否会回到h2头结点。</div>
<div>解法4：最简单的方法，获得H1和H2尾节点，判断他们是否相同。</div>
<div>

<hr />

<strong>3.7 队列中取最大值操作问题</strong></div>
<div><span style="color: #939600;">假设有一个队列，设计一个数据结构和算法，使在队列上去最大值的操作时间复杂度尽可能降低。</span></div>
<div>解法一：使用传统队列，每次取最大值都遍历队列所有元素。</div>
<div>解法二：使用最大堆维护队列中的元素。同时元素中添加一个成员，指向队列中的下一个元素。取最大值操作只需要O(1)，出入队操作是一个最大堆操作需要O(lgN)。</div>
<div><span style="text-decoration: line-through;">编程之美上的解法3是错误的，它使用了一个最大值序列保存最大值逻辑，但对于6，5，4这个序列，他构造的最大值序列中只存储6这一个值，一旦6 pop出去，就无法在进行最大值操作。</span></div>
<div>更新：对于栈来说，使用一个栈来保存他的最大值是正确的，因为栈具有先进后出的特性。如6，5，4序列，只需要存储6，因为如果6pop出去，5和4肯定已经pop出去了，只需要记录下在6之前的序列中的最大值即可。</div>
<div><span style="text-decoration: line-through;">更好的方法是使用一个最大堆来保存最大值逻辑。</span></div>
<div>使用最大堆作为辅助数据结构有缺点，就是存入和取出操作都是O(lgN)。</div>
<div>不过文中还有一个用两个栈实现一个队列的思想，不错。设栈A和B，入队时都将元素存如栈B，出队时将将栈A的元素弹出，如果栈A中没有元素，那么将栈B的元素依次弹出并放入栈A。</div>
<div>

<hr />

<strong>3.8 求二叉树节点的最大距离。</strong></div>
<div><span style="color: #939600;">假设二叉树的边是双向的，定义距离为两个节点之间边的个数。</span></div>
<div>其实这个过程就是寻找一个节点，这个节点左子树深度和右子树深度之和（以这个节点为跟的并且最大距离经过这个节点的节点最大距离）最大。</div>
<div>假设一个节点的左子树和右子树的深度都已经知道了，那么这个节点的深度，已经经过这个节点的最大距离都可以知道。</div>
<div>递归可以实现以上过程。</div>
<div></div>
<div>分析递归问题，编程之美给了三点经验：</div>
<div>1.弄清楚递归的顺序。递归视实现中假设后续调用已经完成，在此基础上实现自身的逻辑。</div>
<div>2.分析递规体的逻辑，即知道了子问题，如何计算这个问题。</div>
<div>3.弄清楚递归退出的边界，也就是说哪些地方要写return。</div>
<div>

<hr />

</div>
<div><strong>3.9 重建二叉树</strong></div>
<div><span style="color: #939600;">根据前序遍历和中序遍历重建二叉树。</span></div>
<div>前序遍历的第一个节点是根节点，然后找到中须遍历的这个节点，中须遍历数组中这个节点的前面的序列就是根节点的左子树，后面的节点序列就是根节点的右子树，递归。</div>
<div>     再注：看到了拓展问题2，明白了这些判断的意义和价值。如果前序遍历和中须遍历给的不合理，这些判断可以判断出来。</div>
<div>扩展问题：</div>
<div>1.如果根据字母不能确定节点，即有些节点的字幕可能相同的。该题会出现什么情况？</div>
<div>答：重构出来的二叉树肯定不唯一。可以将所有的可能的情况都找出来。思想相同，更复杂些。</div>
<div>2.如何判断给定的前序遍历和中序遍历的结果是合理的？</div>
<div>能否根据算法构造出一个对应的二叉树。如前序：acb，中序bac，就没有办法构造出二叉树。</div>
<div>3.如果知道前序和后序遍历的结果，能重构二叉树么？</div>
<div>不能！当一个节点只有左子树或右子树（一个子树）时，无法分辨出来是左还是右子树。</div>
<div></div>
<div></div>
<div>

<hr />

</div>
<div>二叉树的遍历</div>
<div>前序遍历：</div>
<div>PreTraverse(Node *p){</div>
<div>     if(p==NULL) return;</div>
<div>      printf(p-&gt;value);</div>
<div>     if(p-&gt;left!=NULL){</div>
<div>          PreTraverse(p-&gt;left);</div>
<div>     }</div>
<div>     if(p-&gt;right!=NULL){</div>
<div>          PreTraverse(p-&gt;right)</div>
<div>     }</div>
<div>}</div>
<div>中序遍历：</div>
<div>MidTraverse(Node *p){</div>
<div>     if(p==NULL) return;</div>
<div>     if(p-&gt;left!=NULL){</div>
<div>         MidTraverse(p-&gt;left);</div>
<div>     }</div>
<div>      printf(p-&gt;value);</div>
<div>     if(p-&gt;right!=NULL){</div>
<div>         MidTraverse(p-&gt;right)</div>
<div>     }</div>
<div>}</div>
<div>中序遍历不使用递归版本一（只用栈存储遍历状态）</div>
<div>MidTraverse(Node *p){</div>
<div>     if(p==NULL) return;</div>
<div>     stack&lt;Node *&gt; Nstack;</div>
<div>     Nstack.push(p);</div>
<div>     while(!Nstack.isEmpty()){</div>
<div>         tmp_Ptr=Nstack.getTop();</div>
<div>          //如果tmp_Ptr==NULL，说明上一次访问的是一个右子树为NULL的节点</div>
<div>          while(tmp_Ptr!=NULL) Nstack.push(tmp_Ptr-&gt;left);tmp_Ptr=tmp_Ptr-&gt;left;//向左走到尽头最后会将一个NULL指针放在stack的顶端</div>
<div>          Nstack.pop();//将NULL指针去掉</div>
<div>          if(!Nstack.isEmpty()){</div>
<div>               tmp_Ptr=Nstack.pop();</div>
<div>               printf(tmp_Ptr-&gt;value);</div>
<div>               Nstack.push(tmp_Ptr-&gt;right);//如果右指针指向NULL，也将其放入stack的顶端，以标识</div>
<div>          }</div>
<div>     }</div>
<div>}</div>
<div>中序遍历不使用递归版本二，这个更好理解</div>
<div>MidTraverse(Node *T){</div>
<div>     Node *p=T;</div>
<div>     stack&lt;Node*&gt; Nstack;</div>
<div>     while(p||!Nstack.isEmpty()){</div>
<div>          if(p!=Null){</div>
<div>               Nstack.push(p);</div>
<div>               p=p-&gt;left;</div>
<div>          }else{</div>
<div>               p=Nstack.pop();</div>
<div>               printf(p-&gt;value);</div>
<div>               p=p-&gt;right;</div>
<div>          }</div>
<div>     }</div>
<div>}</div>
<div>后序遍历：</div>
<div>AfterTraverse(Node *p){</div>
<div>      if(p==NULL) return;</div>
<div>     if(p-&gt;left!=NULL){</div>
<div>        AfterTraverse(p-&gt;left);</div>
<div>     }</div>
<div>      printf(p-&gt;value);</div>
<div>     if(p-&gt;right!=NULL){</div>
<div>        AfterTraverse(p-&gt;right)</div>
<div>     }</div>
<div>}</div>
<div></div>
<div>

<hr />

<strong>3.10分层遍历二叉树</strong></div>
<div><span style="color: #939600;">问题1：给定一颗二叉树，分层遍历该二叉树。</span></div>
<div>使用队列，输出这个节点的同时将他的左右子节点存入队列，先进先出。同时想法子标识一层的结束用于分行。</div>
<div><span style="color: #939600;">问题2：打印二叉树某层次节点（从左到右），根节点为0层。</span></div>
<div>可使用递归，int printNodeAtLevel(Node *root,int level),若level&gt;0，则递归调用root的左右子节点，并将level-1,当level=0时左右边界条件输出并返回。</div>
<div>拓展问题：如果从下到上访问，咋搞？</div>
<div>使用栈，先访问右节点，在访问左节点，先进后出。打印出来即可。</div>
<div>

<hr />

</div>
<div></div>