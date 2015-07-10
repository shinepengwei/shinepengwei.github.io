---
layout: post
title:  剑指offer 读书笔记
category: "理论及算法"
tag: "读书笔记"
---

<div>每个函数都需要对输入的函数参数进行验证，查看函数用户输入的实参有没有问题。如赋值运算符重载要考虑是否和当前对象相同，链表操作考虑头指针是否为NULL。</div>
<div>小数（如float和double）不可以使用等号进行判断。</div>
<div>使用位操作-与操作作为判断一个整数是奇数还是偶数是一个比较好的方法。</div>
<div></div>
<div>

<hr />

</div>
面试题1 赋值运算符函数
<div>1.返回值类型声明为该类型的引用，然后函数结束前return *this.</div>
<div>2.把传入的参数的类型声明为常量引用。</div>
<div>3.首先释放实力自身已占用的内存。</div>
<div>4.判断输入的参数和当前的实例是否是一个实例</div>
<div>

<hr />

面试题2 实现singleton模式</div>
<div>1.将构造函数声明为private类型。</div>
<div>2.加入一个static的该类的对象指针成员。</div>
<div>3.写一个static函数，如果对象指针不为NULL，返回这个对象指针，否则可以通过这个函数构造对象，并且将对象指针成员指向这个对象，然后返回。</div>
<div>

<hr />

字符串</div>
<div>c/C++通常把常量字符串单独放在一个内存区域。当几个指针赋值给相同的常量字符串时，它们实际上指向相同的内存地址。</div>
<div>char str1[]="hello";</div>
<div>char str2[]="hello";</div>
<div>char *str3="hello";</div>
<div>char *str4="hello";</div>
<div>str1和str2指向不同的地址，而str3和str4只想相同的地址。即str1!=str2，str3==str4</div>
<div>

<hr />

链表</div>
<div>当对链表进行操作，函数需要链表的头节点指针时，需要传入指向头节点指针的指针。引入如果这个链表现在为NULL，那头结点指针为NULL，通过函数参数传值，传进来的是复制头结点指针的一个NULL指针，修改该指针其实并没有真正的修改头指针。</div>
<div>

<hr />

</div>
<div><strong>面试题11 数值的整数次方</strong></div>
<div>实现 double Power(double base,int exponent)</div>
<div>注意：exponent可能是0、负数和正数。</div>
<div>高效的方法：</div>
<div>double Power(double base,int exponent){</div>
<div>     if(exponent==0)</div>
<div>          return 1;</div>
<div>     if(exponent==1)</div>
<div>          return base;</div>
<div>     double result=Power(base,exponent&gt;&gt;1);</div>
<div>     result*=result;</div>
<div>     if(exponent&amp;0x01==1){</div>
<div>          result*=base;</div>
<div>     }</div>
<div>     return result;</div>
<div>}</div>
<div>1.使用右移运算符代替除以2，这里有个疑问：对于负数，右移一位是否也代表着除以2？</div>
<div>int i=-3;</div>
<div>int x=i&gt;&gt;1;</div>
<div>int y=i/2;</div>
<div>cout&lt;&lt;x&lt;&lt;","&lt;&lt;y&lt;&lt;endl;</div>
<div>结果为-2，-1.由此可见，对于负数右移和除2两码事。右移是向下取正，而除2是忽略掉小数，相当于向上取正。</div>
<div>2.使用位的与运算来代替取余判断一个数是奇数还是偶数。</div>
<div>剑指Offer给的函数，但是这个函数没有考虑exponent&lt;0的情况。</div>
<div>更改如下：</div>
<div>double Power(double base,int exponent){</div>
<div>     <span style="color: #e30000;">if(exponent&lt;0){</span></div>
<div><span style="color: #e30000;">          return 1/Power(base,0-exponent);</span></div>
<div><span style="color: #e30000;">     }</span></div>
<div>     if(exponent==0)</div>
<div>          return 1;</div>
<div>     if(exponent==1)</div>
<div>          return base;</div>
<div>     double result=Power(base,exponent&gt;&gt;1);</div>
<div>     result*=result;</div>
<div>     if(exponent&amp;0x01==1){</div>
<div>          result*=base;</div>
<div>     }</div>
<div>     return result;</div>
<div>}</div>
<div>

<hr />

面试题12 打印1到最大的N位数</div>
<div>要考虑当N非常大无法用内置类型表示的情况。</div>
<div>因为是要输出，我们考虑使用字符串模拟N位数。</div>
<div>有两种方法：</div>
<div>1.字符串上模拟数字加法</div>
<div>2.转换为数字排列的解法，递归。此方法更加简洁。</div>
<div>void print1ToMaxOfNDigit(int n){</div>
<div><span style="color: #147191;">     if(n&lt;=0) return;//当用户输入不符合规则时，报错返回。</span></div>
<div>     char *num=new char[n+1];</div>
<div>     for(int i=0;i&lt;n;i++) num[i]='0';</div>
<div><span style="color: #147191;">    num[n]='\0';</span></div>
<div>     for(int i=0;i&lt;n;i++){</div>
<div>          print1ToMaxOfNDigitRecursively(num,1,n);</div>
<div>     }</div>
<div>  <span style="color: #147191;">   delete[] num;//每次不要忘了对内存进行释放</span></div>
<div>}</div>
<div>void print1ToMaxOfNDigitRecursively(char *num,int index,int n){</div>
<div>     if(index==n-1){</div>
<div>          print(num);</div>
<div>     }else{</div>
<div>          for(int i=0;i&lt;10;i++){</div>
<div>               number[index]=i+'0';</div>
<div>               print1ToMaxOfNDigitRecursively(num,index+1,n);</div>
<div>          }</div>
<div>     }</div>
<div>}</div>
<div><span style="color: #e30000;">注意：题目关于N位数的整数并且没有限定N的取值范围，或者输入任意大小的整数，就需要考虑大数问题。字符串是一个简单有效的表示大数的方法。</span></div>
<div>

<hr />

</div>
<div>面试题13 在O(1)时间内删除链表节点</div>
<div>给定单链表的头结点和一个节点指针，定义一个函数在O(1)时间内删除节点指针指向的节点。</div>
<div>方法：删除该节点指针指向的下一个节点，然后把下一个节点的内容复制到这个节点。</div>
<div>要注意：</div>
<div>1.函数不能传递头结点指针作为参数，而要获得指向头结点指针的指针。</div>
<div>2.考虑当链表只有一个节点头结点时的情况。</div>
<div>3.考虑节点为尾节点，下一个节点为NULL的情况。</div>
<div>4.考虑节点指针指向的节点不属于这个链表。这种情况O(1)函数肯定无法完成。</div>
<div>5.头结点和节点指针都为NULL，链表空的情况。</div>
<div>

<hr />

</div>
<div>面试题14 调整数组的顺序使奇数位于偶数前面</div>
<div>使用连个指针，一个指向头部，一个指向尾部。当头部指针元素为偶数，尾部指针元素为奇数时，交换这两个元素，并且使他们朝中间靠拢。否则，令不符合要求的指针向中间靠拢。</div>
<div>

<hr />

</div>
<div>面试题14 链表中倒数第k个节点</div>
<div>令一个指针P1移动K-1步，然后令P2指向头结点，这时使P1和P2同时向后一起遍历，当P1指向最后一个节点的时候，P2指向的就是倒数第K各节点</div>
<div>同时要考虑算法的健壮性，以下情况可能使函数崩溃：</div>
<div>1.链表为空。</div>
<div>2.链表的个数小于K。</div>
<div>3.输入的参数K&lt;=0。</div>
<div></div>
<div><span style="color: #e30000;">使用两个指针解决链表问题：</span></div>
<div>1。求链表的中间结点：两个节点首先都指向头结点，一个每次移动一次，一个每次移动两次，移动两次的指针指向尾节点时，移动一次的指针指的就是中间结点。</div>
<div>2.判断单项链表是否存在环形结构：两个节点指向头结点，一个每次移动一次，一个每次移动两次。若移动两次的指针追上了移动一次的指针，则说明有环形。如果走的快的走到了链表末尾，则链表不是环形链表。</div>
<div>举一反三：当用一个指针遍历链表无法解决问题的时候，考虑使用两个指针遍历，可以让某一个更快一些。</div>
<div>比如删除某一节点，其实就是一前一后。</div>
<div>

<hr />

面试题18 树的子结构。判断数A的一部分子结构是否是和B一样。</div>
<div>递归方法，书中给了两个函数，函数一递归调用每个A中的节点N和B的根节点是否相同，相同的话使用函数二判断以N为根的树是否直接包含了B树。如果N节点和B的根节点不同，再递归调用函数一判断A的左右子树是否包含B树。</div>
<div>我自己写的程序，只有一个递归函数，但是引入一个<a>标志变量区分实现书中的两</a>个函数的不同功能。</div>
<div>书中给的程序更加易懂。</div>
<div>注意：二叉树代码有大量的指针操作，也常常使用递归算法，因此每次在使用指针的时候，都要考虑指针是否可能为NULL，如果为NULL要怎么处理。</div>
<div>

<hr />

面试题21 包含min函数的栈</div>
<div>定义栈的结构，并且要求在这个栈上调用取最小元素min、push和pop函数的时间复杂度都为O(1)。</div>
<div>为栈定义一个辅助栈，保存栈当前的最小元素。因为栈符合先进后出的要求，所以是可以的。</div>
<div>对于队列呢，如果要在队列上获得这个结果，可以使用两个栈来模拟一个队列的行为。</div>
<div>

<hr />

面试题24 二叉搜索树的后序遍历序列</div>
<div>输入一个数组，判断该数组是不是某二叉搜索数的后序遍历结果。</div>
<div><span style="text-decoration: underline;">二叉搜索树</span>：或者是一个空树，或者是一个具有以下性质的二叉树：若他的左子树不空，则左子树上的所有节点都小于他的根节点。如果右子树不空，则右子树上的所有节点都大于根节点。左右子树也是一个二叉排序树。</div>
<div>在二叉搜索树的后序遍历序列中，最后一个数字为树的根节点的值。数组中前面的数字可以分为两部分，第一部分是左子树节点，他们比根节点小。第二部分是右子树节点，他们比根节点大。</div>
<div>

<hr />

面试题26 复杂链表的复制</div>
<div>复杂链表表示链表节点中有一个指针指向下一个节点，还有一个指针随机指向任意节点。</div>
<div>这种复杂连表的复制可以分为两步，第一步复制每个节点，并且同时复制指向下一个节点的指针，就是和复制普通链表一样。第二步复制节点中指向任意节点的指针。第一步的方法都是一样的，区别在于第二步。</div>
<div>第二步有三种方法：</div>
<div>第一种：在复制每个节点的只想任意节点的指针时，从头遍历去确定指向的节点与头结点的距离，然后以此在复制的链表中修改相应指针。这种方法的时间复杂度为O(N*N)</div>
<div>第二种：在第一步时同时存储原节点和复制节点的映射关系，使用hash表记录下来，然后去查找相应的节点复制指针。这样时间复杂度为O(N)，但是需要O(N)的空间复杂度的hash表。</div>
<div>第三种：将复制的节点插入在被复制节点的后面，使复制链表和被复制链表变成一个链表，然后根据被复制链表中节点的指向任意节点的指针去修改被复制节点的对应指针。全部修改完成后然后将这个链表拆分。这样时间复杂度为O(N)，而且不需要额外的空间。</div>
<div>

<hr />

面试题27 字符串的排列</div>
<div>如输入abc，打印abc所能组合的所有字符串，如abc,acb,bac,bca,cab,cba</div>
<div>方法：看成两步：</div>
<div>第一步：所有可能出现在第一个位置的字符，即把第一个字符和后面包括自身所有的字符交换。</div>
<div>第二步：固定第一个字符，求后面所有字符的排列情况。（递归）</div>
<div>扩展问题——组合问题：</div>
<div align="left"><span style="color: #0000ff; font-family: 'Times New Roman'; font-size: xx-small;">char</span> <span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">*</span><span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">p</span> <span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">=</span><span style="color: #a31515; font-family: 'Times New Roman'; font-size: xx-small;">"abcd"</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">;</span></div>
<div align="left"><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">  </span> <span style="color: #0000ff; font-family: 'Times New Roman'; font-size: xx-small;">char</span> <span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">tmp</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">[5];</span></div>
<div align="left"><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">  </span> <span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">zuhe</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">(</span> <span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">p</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">,</span><span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">tmp</span> <span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">,0,0)</span></div>
<div align="left"><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">
</span></div>
<div align="left"><span style="color: #0000ff; font-family: 'Times New Roman'; font-size: xx-small;">void</span> <span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">zuhe</span> <span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">(</span><span style="color: #0000ff; font-family: 'Times New Roman'; font-size: xx-small;">char</span> <span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">*</span> <span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">str</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">,</span><span style="color: #0000ff; font-family: 'Times New Roman'; font-size: xx-small;">char</span> <span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">*</span><span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">outStr</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">,</span> <span style="color: #0000ff; font-family: 'Times New Roman'; font-size: xx-small;">int</span> <span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">sBegin</span> <span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">,</span><span style="color: #0000ff; font-family: 'Times New Roman'; font-size: xx-small;">int</span> <span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">outBegin</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">){</span></div>
<div align="left"><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">   </span> <span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">outStr</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">[</span><span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">outBegin</span> <span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">]=</span><span style="color: #a31515; font-family: 'Times New Roman'; font-size: xx-small;">'\0'</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">;</span></div>
<div align="left"><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">   </span> <span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">cout</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">&lt;&lt;</span><span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">outStr</span> <span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">&lt;&lt;</span><span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">endl</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">;</span></div>
<div align="left"><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">   </span> <span style="color: #0000ff; font-family: 'Times New Roman'; font-size: xx-small;">int</span> <span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">p</span> <span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">=</span><span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">sBegin</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">;</span></div>
<div align="left"><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">   </span> <span style="color: #0000ff; font-family: 'Times New Roman'; font-size: xx-small;">while</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">(</span><span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">str</span> <span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">[</span><span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">p</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">]!=</span> <span style="color: #a31515; font-family: 'Times New Roman'; font-size: xx-small;">'\0'</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">){</span></div>
<div align="left"><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">       </span> <span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">outStr</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">[</span><span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">outBegin</span> <span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">]=</span><span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">str</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">[</span> <span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">p</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">];</span></div>
<div align="left"><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">       </span> <span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">zuhe</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">(</span><span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">str</span> <span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">,</span><span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">outStr</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">,</span> <span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">p</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">+1,</span><span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">outBegin</span> <span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">+1);</span></div>
<div align="left"><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">        ++</span> <span style="color: #010001; font-family: 'Times New Roman'; font-size: xx-small;">p</span><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">;</span></div>
<div align="left"><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">    }</span></div>
<div align="left"><span style="color: #010101; font-family: 'Times New Roman'; font-size: xx-small;">}</span></div>
<div>相关题目：</div>
<div>1.输入一个含有八个数字的数组，判断有没有可能把这8个数字分别放在正方体八个顶点，使其三组相对的面上的四个顶点的和相等。</div>
<div>2.8皇后问题。</div>
<div>这两个题目可以通过排列，并且对排列结果进行判断。</div>
<div>如果要求按照一定的要求摆放若干数字，可以尝试对这些数字进行排列，然后一一判断这些排列是否符合要求。</div>
<div>

<hr />

面试题29 数组中出现次数超过一半的数字</div>
<div>《编程之美》中给出了一个解决方法，就是每次从数组中删掉两个不同的数字，最后留下来的就是要找的。</div>
<div>《剑指Offer》中给出了另一个方法，即基于Partition函数，求第N/2大的数字。求数组中任意第K大的数字有一个成熟的O(n)算法。</div>
<div>注意：求数组中任意K大的数字的成熟的O(N)算法是基于快速排序的思想的，这种方法肯定要对输入数组进行修改。</div>
<div>

<hr />

面试题30 最小的k个数</div>
<div>很多种方法，编程之美也有详细论述。</div>
<div>这里注意：</div>
<div>1.使用基于快速排序的方法是O(N)的时间复杂度，但是要求对输入数组进行修改。</div>
<div>

<hr />

面试题33 把数组排成最小的数</div>
<div>对于一个整形数组，把所有的数字排成一个数，让这个数取最小的值。</div>
<div><span style="color: #e30000;">这个题目希望我们找到一个排序规则，然后数组按照这个排序规则排序从而获得一个最小的数字。</span></div>
<div>这个排序规则就是比较两个数，如m、n，我们需要判断m和n哪个应该放在前面。我们可以这样比较，如果mn&gt;nm则n应该放在前面，否则m放在前面。综上所述，我们定义了一个新的比较判断规则，然后使用排序算法就可以获得我们希望要的取值。</div>
<div>注意，我们在判断mn和nm的时候，需要考虑他是否是计算机可以表示的int型，因此，这里隐含其实是一个大数问题，大数问题一般可以使用字符串解决。</div>
<div>

<hr />

面试题34 丑数</div>
<div>把只含因子2、3、5的数称为丑数。要求按从小到大的顺序获得的1500个丑数。</div>
<div>方法1：逐个判断每个整数是不是丑数，方法简单，但是不高效。</div>
<div>方法2：根据已有的丑数数组，计算随后的丑数。假设目前的丑数数组中最大的值为M，我们计算数组中从小到大所有的值的乘以2、3、5第一个大于M的数，并且比较，取最小的作为下一个丑数。同时，我们可以记录T2、T3、T5保存丑数数组中已经计算过2、3、5的元素。这样每次只需要朝下计算即可。</div>
<div>

<hr />

题目35 第1次只出现一次的字符</div>
<div>使用hash表，key是字符，value存储每个字符出现的次数。第一次遍历字符串记录每个字符出现的次数，第二次遍历找到只出现一次的字符。</div>
<div>

<hr />

题目36 数组中的逆序对</div>
<div>在数组中的两个数字如果前面的一个数字大于后面的数字，则这两个数组组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。</div>
<div>方法1：扫描每个数组然后和他后面的数字比较大小，然后统计逆序对，时间复杂度为O(n^2)。</div>
<div>方法2：将数组分隔为子数组，先统计出字数组中逆序对，并且这个过长会对子数组从小到达排序；然后在统计处两个相邻数组之间的逆序对数量，统计方法略，和递归排序有点像。</div>
<div>

<hr />

题目37 两个链表的第一个公共节点</div>
<div>方法1：可以使用两个栈，每个栈保存一个链表，最后从每个站弹出节点，最后一个一样的节点就是第一个公共节点。</div>
<div>方法2：可以使用hash表，把每个节点的地址作为key，查找是否出现。</div>
<div>方法3：最优秀的方法，不需要使用额外存储空间。计算每个链表的长度，让那个更长的链表先走几步使他们一样长，然后开始每个链表每次走一步，然后比较是否相等。</div>
<div>

<hr />

题目38 数字在排序数组中出现的次数</div>
<div>使用两次二分查找查找到第一个出现的位置和最后一个出现的位置，然后之差+1即可，时间复杂度为O(lgN)</div>
<div>

<hr />

</div>
<div>题目39 二叉树的深度</div>
<div>使用递归</div>
<div>int depth(Node * p){</div>
<div>     if(p==Null) return 0;</div>
<div>     int depLeft=depth(p-&gt;left);</div>
<div>     int depRight=depth(p-&gt;right);</div>
<div>     return max(depLeft,depRight)+1;</div>
<div>}</div>
<div>

<hr />

</div>
<div>拓展问题：判断一个二叉树是否是平衡二叉树。平衡二叉树的任意节点的左右子树的深度差不超过1。</div>
<div>方法1：直接求每个节点的左右子树的深度，然后进行判断。这种方法存在大量的重复计算</div>
<div>方法2：自下而上判断每个节点是否左右子树的深度差不超过1，同时返回该节点的深度</div>
<div>

<hr />

面试题40 数组中只出现一次的数字</div>
<div><span style="color: #939600;">一个整形数组，除了两个数之外，其他的数字都出现了两次，找出这两个数字，要求时间复杂度O(N)，空间复杂度o(1).</span></div>
<div>先考虑如果一个数组中只有一个数出现了一次，其他都是两次，怎么找到这个数？这里用到了异或操作。</div>
<div>异或的性质：任何一个数字异或他自己都等于0.也就是说，我们从头到尾异或数组的没一个数字，最终结果等于只出现一次的数字。</div>
<div>对于一个数组中出现一次的个数为2，那我们可以先将数组中所有的数异或，然后获得了一个结果数，这个结果肯定不为0，则某一位为1，由于异或的性质可以得知，那两个出现一次的数的异或就为结果，同时说明为1的那一位是两个数一个为1，一个为0.因此，我们根据这一位为0还是为1将这个数组分为两个子数组，这样那两个出现一次的数肯定分别在两个数组中，然后在这两个数组中分别获得出现次数为1的数即可。</div>
<div>

<hr />

面试题41 和为s的两个数字   VS  和为s的连续正数序列 VS 和为s的任意个小于n的数</div>
<div>题目1 输入一个递增数组和一个数字s，在数组中查找两个数使他们的和正好为s。</div>
<div>使用两个指针，从两头朝中间靠拢，来判断能否找到和为s的两个数字。</div>
<div></div>
<div>题目2 输入一个正数s，打印出所有和为s的连续正数序列（至少两个数）。</div>
<div>使用上一题目思想，使用两个标识small和big，令big+1直至和大于s，再领small+1直至小于s，循环直至small&gt;s/2。这时候说明不可能找到序列使他们的和为s，肯定都大于s。</div>
<div></div>
<div>题目3 输入两个整数s和n，从数列中1，2，3。。。。n中随意取几个数，使其和为s，将所有的组合列出来。</div>
<div>解法1：</div>
<div>
<ol start="1">
	<li>list&lt;int&gt;list1;</li>
	<li>void find_factor(int sum, int n)</li>
	<li>{</li>
	<li>    // 递归出口</li>
	<li>    if(n &lt;= 0 || sum &lt;= 0)</li>
	<li>        return;</li>
	<li></li>
	<li>    // 输出找到的结果</li>
	<li>    if(sum == n)</li>
	<li>    {</li>
	<li>        // 反转list</li>
	<li>        list1.reverse();</li>
	<li>        for(list&lt;int&gt;::iterator iter = list1.begin(); iter != list1.end(); iter++)</li>
	<li>            cout &lt;&lt; *iter &lt;&lt; " + ";</li>
	<li>        cout &lt;&lt; n &lt;&lt; endl;</li>
	<li>        list1.reverse();</li>
	<li>    }</li>
	<li></li>
	<li>    list1.push_front(n);      //典型的01背包问题</li>
	<li>    find_factor(sum-n, n-1);   //放n，n-1个数填满sum-n</li>
	<li>    list1.pop_front();</li>
	<li>    find_factor(sum, n-1);     //不放n，n-1个数填满sum</li>
	<li>}</li>
</ol>
</div>
<div>解法2：该题目属于子集和问题，可以采用回溯法+剪枝<a href="http://blog.csdn.net/v_JULY_v/article/details/6419466">http://blog.csdn.net/v_JULY_v/article/details/6419466</a></div>
<div>

<hr />

面试题42 翻转单次顺序  VS  左旋转字符串</div>
<div>题目1 翻转单词顺序——两次翻转法</div>
<div>题目2 左旋转字符串——三次旋转：翻转前n个，翻转后m个，翻转所有个。</div>
<div>

<hr />

面试题43 圆圈中最后剩下的数字</div>
<div>题目：0,1,...,n-1这n个数字排成一个圆圈，从数字0开始每次从圆圈中删除第m个数字，求这个圆圈中剩下的最后一个数字。</div>
<div>方法1:使用链表模拟这个圆圈，每次去掉一个第m个数字，最后链表剩下了一个就所要的值。</div>
<div>方法2:定义f(n,m)为n个数字中删除第m个数字最后剩下的数字。当第一次删除第k个数字后，我们可以将k+1,k+2,...0,1,...k-1映射为0~n-2，映射p(x)=(x-k-1)%n，逆映射为p*(x)=(x+k+1)%n.则f(n,m)=p*[f(n-1,m)]=(f(n-1,m)+k+1)%n,由于k=(m-1)%n，则f(n,m)=[f(n-1,m)+m]%n.同时，当n=<a>1时</a>，只有一个数字0，则就是最后剩下的数字。</div>
<div>

<hr />

面试题46 求1+2+...+n，不能使用乘除法、for/while/if/else/switch/case等关键词和条件判断语句。</div>
<div>1.使用构造函数。</div>
<div>2.使用虚函数。</div>
<div>3.是用函数指针</div>
<div>4.使用模版类型</div>
<div>

<hr />

</div>
<div>面试题47 不用加减乘除做加法</div>
<div>使用位运算完成加法。</div>
<div>第一步：不考虑进位对每一位相加（异或）</div>
<div>第二步：考虑进位，使用与获得进位的位，然后左移一位进行异或运算。</div>
<div>第三部：重复以上两步，直至进位为0.</div>
<div></div>
<div>拓展：不使用新的变量，交换两个变量。</div>
<div>1.加减法2.异或法</div>
<div>

<hr />

面试题48 不能被继承的函数</div>
<div>1.把构造函数设为私有函数。然后定义一个产生指向对象的指针的static函数和相应的析构函数。</div>
<div></div>