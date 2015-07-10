---
layout: post
title: 推荐系统综述
category: "理论及算法"
tag: "LBSN及推荐"
---
<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;">推荐系统目前用途广泛，但是仍然处在一些不足需要改进。</span>
</span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;"><strong>推荐模型</strong></span>
</span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;">用户向推荐主动提供个人偏好信息或推荐请求（也可不主动提供而让推荐系统主动采集），推荐系统根据不同的推荐策略或根据已建模的知识库进行推荐，返回推荐结果。</span>
</span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: 宋体;">推荐根据用户和推荐对象（</span><span style="font-family: Tahoma;">item</span><span style="font-family: 宋体;">）的信息对未评价的</span><span style="font-family: Tahoma;">item</span><span style="font-family: 宋体;">进行评价：</span></span><span style="font-family: 宋体;">
</span></span>

<img src="http://www.codingart.info/wp-content/uploads/2012/09/090112_0318_1.png" alt="" /><span style="font-family: 宋体; font-size: 14pt;">
</span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;"><strong>现有的推荐算法</strong></span>
</span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;">推荐算法有不同的分类，比较细致的分类可分为以下几类：</span>
</span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">1 </span><span style="font-family: 宋体;">基于内容的推荐</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: 宋体;">基于内容的推荐是通过匹配</span><span style="font-family: Tahoma;">item</span><span style="font-family: 宋体;">的特征和用户兴趣的特征的相关性，认为相关性高的</span><span style="font-family: Tahoma;">item</span><span style="font-family: 宋体;">用户更可能感兴趣。</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: 宋体;">通常用户的兴趣根据用户的历史数据获得，如用户已评价的</span><span style="font-family: Tahoma;">item</span><span style="font-family: 宋体;">的特征。</span></span><span style="font-family: 宋体;">
</span></span>

<img src="http://www.codingart.info/wp-content/uploads/2012/09/090112_0318_2.png" alt="" /><span style="font-family: 宋体; font-size: 14pt;">
</span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;">缺点：</span>
</span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: 宋体;">特征提取对于某些</span><span style="font-family: Tahoma;">item</span><span style="font-family: 宋体;">如多媒体来说很难，有些特征相同的</span><span style="font-family: Tahoma;">item</span><span style="font-family: 宋体;">对用户来说具有较大的差异（比如我这篇文章和</span><span style="font-family: Tahoma;">IEEE transaction</span><span style="font-family: 宋体;">那篇文章都是推荐系统的综述文章，但是估计没几个人愿意看我的）。</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;">无法为用户推荐其他方面的文章。</span>
</span>

<span style="color: black; font-size: 14pt;"><span style="font-family: 宋体;">新</span><span style="font-family: Tahoma;">User</span><span style="font-family: 宋体;">问题。</span><span style="font-family: Tahoma;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">2 </span><span style="font-family: 宋体;">协同过滤</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;">协同过滤技术是根据相似用户的喜好为用户进行推荐。，是目前应用最广泛的推荐技术。</span>
</span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">1) </span><span style="font-family: 宋体;">启发式算法</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: 宋体;">启发式算法的思想是使用用户</span><span style="font-family: Tahoma;">c</span><span style="font-family: 宋体;">的相似用户</span><span style="font-family: Tahoma;">c'</span><span style="font-family: 宋体;">对一个对象</span><span style="font-family: Tahoma;">s</span><span style="font-family: 宋体;">的评价来预测用户</span><span style="font-family: Tahoma;">c</span><span style="font-family: 宋体;">对对象</span><span style="font-family: Tahoma;">s</span><span style="font-family: 宋体;">的评价已决定是否推荐。</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;">用户的相似度的计算主要是根据两个用户对同意对象的评分差异。最近本的两种方法是基于关联的和基于余弦距离的。</span>
</span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: 宋体;">基于关联的是根据用户</span><span style="font-family: Tahoma;">c</span><span style="font-family: 宋体;">和用户</span><span style="font-family: Tahoma;">c'</span><span style="font-family: 宋体;">共同评价的所有</span><span style="font-family: Tahoma;">item</span><span style="font-family: 宋体;">的评价相似度计算关联。</span></span><span style="font-family: 宋体;">
</span></span>

<img src="http://www.codingart.info/wp-content/uploads/2012/09/090112_0318_3.png" alt="" /><span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">
</span><span style="font-family: 宋体;">基于余弦距离的方法直接把评分作为向量来计算余弦距离。</span></span><span style="font-family: 宋体;">
</span></span>

<img src="http://www.codingart.info/wp-content/uploads/2012/09/090112_0318_4.png" alt="" /><span style="font-family: 宋体; font-size: 14pt;">
</span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;">预测评价的计算公式如下表示：</span>
</span>

<img src="http://www.codingart.info/wp-content/uploads/2012/09/090112_0318_5.png" alt="" /><span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: 宋体;">，</span><span style="font-family: Tahoma;">aggr</span><span style="font-family: 宋体;">表示相似用户评价的启发式函数。</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;">启发式函数主要有三种：</span>
</span>

<img src="http://www.codingart.info/wp-content/uploads/2012/09/090112_0318_6.png" alt="" /><span style="font-family: 宋体; font-size: 14pt;">
</span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: 宋体;">其中，</span><span style="font-family: Tahoma;">(a)</span><span style="font-family: 宋体;">简单的取平均值，</span><span style="font-family: Tahoma;">(b)</span><span style="font-family: 宋体;">根据相似度取期望值，</span><span style="font-family: Tahoma;">(c)</span><span style="font-family: 宋体;">引入了归一化变量，杜绝了用户之间的评价标准（评价平均值）不同。</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">2) </span><span style="font-family: 宋体;">基于模型的方法</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: 宋体;">此方法利用用户</span><span style="font-family: Tahoma;">c</span><span style="font-family: 宋体;">对众多对象的评价来学习</span><span style="font-family: Tahoma;">c</span><span style="font-family: 宋体;">的模型，把用户归类到一个模型或者一个类型中，然后使用概率的方法对新的对象</span><span style="font-family: Tahoma;">s</span><span style="font-family: 宋体;">的评价进行预测。形式化表述如下：</span></span><span style="font-family: 宋体;">
</span></span>

<img src="http://www.codingart.info/wp-content/uploads/2012/09/090112_0318_7.png" alt="" /><span style="font-family: 宋体; font-size: 14pt;">
</span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: 宋体;">公式表示对于</span><span style="font-family: Tahoma;">item</span><span style="font-family: 宋体;">的评价的期望值。评价范围为</span><span style="font-family: Tahoma;">0~n</span><span style="font-family: 宋体;">，</span><span style="font-family: Tahoma;">Pr(r=i)</span><span style="font-family: 宋体;">表示评价为</span><span style="font-family: Tahoma;">i</span><span style="font-family: 宋体;">的概率。</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;">缺点：</span>
</span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;">新的用户</span>
</span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: 宋体;">新的</span><span style="font-family: Tahoma;">item</span></span><span style="font-family: 宋体;">
</span></span>

<span style="color: black; font-size: 14pt;"><span style="font-family: 宋体;">评价数据的稀疏性。</span><span style="font-family: Tahoma;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">3 </span><span style="font-family: 宋体;">基于人口统计信息</span></span><span style="font-family: 宋体;">
</span></span>

<span style="color: black; font-size: 14pt;"><span style="font-family: 宋体;">此方法是在推荐系统中引入了用户人口统计信息，如年龄、性别等。</span><span style="font-family: Tahoma;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">4 </span><span style="font-family: 宋体;">基于知识的推荐</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;">此方法是一种推理技术，利用针对特定领域的规则来进行推理。如认为中国人喜欢吃中国菜。</span>
</span>

<span style="color: black; font-size: 14pt;"><span style="font-family: 宋体;">但这种技术的瓶颈在于知识和规则的获取。</span><span style="font-family: Tahoma;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">5 </span><span style="font-family: 宋体;">基于社区的推荐</span></span><span style="font-family: 宋体;">
</span></span>

<span style="color: black; font-size: 14pt;"><span style="font-family: 宋体;">引入用户的社会关系，根据用户的好友的偏好获知用户的偏好。</span><span style="font-family: Tahoma;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">6 </span><span style="font-family: 宋体;">基于组合的推荐</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;">组合推荐通过组合各种推荐技术以弥补某一推荐技术的缺点，根据组合方法可分为三类：</span>
</span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">1)</span><span style="font-family: 宋体;">分别使用不同推荐方法获得推荐结果，结合不同的推荐结果获得最终的推荐结果。（也可以对不同的推荐结果进行评价将最好的推荐结果作为最终的推荐结果）</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">2)</span><span style="font-family: 宋体;">以一种推荐方法为框架，融合令一种推荐方法。</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">3)</span><span style="font-family: 宋体;">基于不同的推荐技术构建一个推荐模型</span></span><span style="font-family: 宋体;">
</span></span>

<img src="http://www.codingart.info/wp-content/uploads/2012/09/090112_0318_8.png" alt="" /><span style="font-family: 宋体; font-size: 14pt;">
</span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;"><strong>推荐系统的性能评价方法和试验方法</strong></span>
</span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: 宋体;">对于推荐系统存在多种多样的评价参数，最常用的是精确度（</span><span style="font-family: Tahoma;">accuracy</span><span style="font-family: 宋体;">）和覆盖度（</span><span style="font-family: Tahoma;">coverage</span><span style="font-family: 宋体;">）。覆盖度是评价推荐系统可推荐的</span><span style="font-family: Tahoma;">item</span><span style="font-family: 宋体;">的覆盖范围。精确度常用的指标有基于统计的和基于决策的。基于统计是比较预测的评价和真实的评价的误差，如（</span><span style="font-family: Tahoma;">mean absolute error</span><span style="font-family: 宋体;">）</span><span style="font-family: Tahoma;">MAE</span><span style="font-family: 宋体;">，</span><span style="font-family: Tahoma;">root mean square error</span><span style="font-family: 宋体;">（</span><span style="font-family: Tahoma;">RMSE</span><span style="font-family: 宋体;">）和</span><span style="font-family: Tahoma;">correlation</span><span style="font-family: 宋体;">。基于决策是评价推荐系统推荐的结果和用户需要的相关程度，如精度、召回率，</span><span style="font-family: Tahoma;">F-measure</span><span style="font-family: 宋体;">和</span><span style="font-family: Tahoma;">ROC</span><span style="font-family: 宋体;">等。</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: 宋体;">试验方法可分为离线实验（</span><span style="font-family: Tahoma;">offline experiment</span><span style="font-family: 宋体;">），用户模拟（</span><span style="font-family: Tahoma;">User Studies</span><span style="font-family: 宋体;">）和在线评价</span><span style="font-family: Tahoma;">(Online Evaluation)</span><span style="font-family: 宋体;">。离线实验是通过已有的数据集进行模拟用户行为。</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: 宋体;">用户模拟是通过让一些试验志愿者对系统进行使用、反馈和评价。在线评价是将系统</span><span style="font-family: Tahoma;">launch</span><span style="font-family: 宋体;">，让真正的用户使用，通过统计数据进行评价。</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: 宋体;">这三种方法的</span><span style="font-family: Tahoma;">cost</span><span style="font-family: 宋体;">以此增加，离线实验是最好的也是最有说服力的，但是往往找不到合适的数据集，这也是很多研究的瓶颈。</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-family: 宋体; font-size: 14pt;"><span style="color: black;"><strong>最新研究方向</strong></span>
</span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">1 </span><span style="font-family: 宋体;">用户和</span><span style="font-family: Tahoma;">item</span><span style="font-family: 宋体;">的特征提取</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">2 </span><span style="font-family: 宋体;">基于情境的推荐</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">3 </span><span style="font-family: 宋体;">多尺度</span><span style="font-family: Tahoma;">(Multcriteria Rating)</span><span style="font-family: 宋体;">推荐技术</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">4 </span><span style="font-family: 宋体;">用户交互的侵袭性</span><span style="font-family: Tahoma;">(Intrusiveness)</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">5 </span><span style="font-family: 宋体;">推荐系统的安全性</span></span><span style="font-family: 宋体;">
</span></span>

<span style="font-size: 14pt;"><span style="color: black;"><span style="font-family: Tahoma;">6 </span><span style="font-family: 宋体;">推荐系统的灵活性</span></span><span style="font-family: 宋体;">
</span></span>

<span style="color: black; font-size: 14pt;"><span style="font-family: Tahoma;">7 </span><span style="font-family: 宋体;">推荐系统的评价准则</span><span style="font-family: Tahoma;">
</span></span>

<span style="color: black; font-family: Tahoma; font-size: 14pt;">Reference
</span>

<span style="color: black; font-family: Tahoma; font-size: 14pt;">[1] Adomavicius, G., Toward the Next Generation of Recommender systems: A Survey of the State-of-the-Art and Possible Extensions. IEEE TRANSAC TIONS ON KNOWLEDGE AND DATA ENGINE ERING, 2005.
</span>

<span style="color: black; font-size: 14pt;"><span style="font-family: Tahoma;">[2] </span><span style="font-family: 宋体;">许海玲等</span><span style="font-family: Tahoma;">, </span><span style="font-family: 宋体;">互联网推荐系统比较研究</span><span style="font-family: Tahoma;">. </span><span style="font-family: 宋体;">软件学报</span><span style="font-family: Tahoma;">, 2009.</span></span>
