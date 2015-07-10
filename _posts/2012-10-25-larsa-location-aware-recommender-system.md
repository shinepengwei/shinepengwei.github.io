---
layout: post
title: LARS:A Location-Aware Recommender System 笔记
category: "理论及算法"
tag: "LBSN及推荐"

---
<p>文章介绍了一个位置感知的推荐系统，将推荐系统分为三类：spatial ratings for non-spatial items, non-spatial ratings for spatial items 和 spatial rating for spatial items。其中spatial rating代表着打分的用户有一个位置属性，既可以理解为哪里打分（移动用户），其实可以理解为用户是哪里人（非移动用户）。Spatial  items表示item是一个location，比如餐厅、电影院等。对于spatial rating文中提出了用户划分(user partitioning)的方法，根据用户所属的位置构建CF模型。对于spatial item提出了移动惩罚(travel penalty)，即考虑用户现在的位置和推荐的位置之间的距离。
</p><h1>1 想法与思考
</h1><p>该文章中提出的用户划分的方法很容易导致更大的稀疏性，文中使用数据集是找的比较密集的数据。
</p><h1>2 用户划分(user partitioning)
</h1><p>该技术主要解决spatial ratings的问题，对具有地理位置属性的user进行建模。
</p><p>文章通过对Movielens和Foursquare的数据进行分析后发现，不同的地域的用户有着不同的兴趣，因此提出了兴趣地域性（preference locality）。因此，使用用户划分的方法将用户根据其家的位置划分，推荐时使用该用户的邻居的评价进行计算推荐评分。<span style="background-color:yellow">但是，我觉得该方法会造成更大<img align="right" src="http://www.codingart.info/wp-content/uploads/2012/10/102512_0306_LARSALocati1.png" alt=""/>的评价的稀疏性。</span>
	</p><p>用户划分中使用自适应金字塔结构，将区域分成金字塔型的层次结构，当位置在区域R中的用户请求推荐时，我们仅使用属于R区域内的打分进行推荐。
</p><p>在构建自适应金字塔结构时，根据scalability和locality的权衡，对区域进行分割或合并，从而获得一个偏(partial)金字塔结构，如右图所示。
</p><p>因为多创建一个区域就要创建多一个对应的CF模型，从而需要更多的存储空间，因此将区域过分的划分可能带来scalability的问题；而如果不划分，无法体现兴趣地域性，又带来了locality的问题，因此随着两个属性的权衡，就形成了一个偏金字塔的数据结构。
</p><p>随着新数据的产生，该结构也需要不断的修改，当对于某一个区域获得了N%的新评价时，就需要对其根据scalability和locality进行权衡判断，并进行合并或分割。合并和分割算法文中有详细介绍，我就不介绍了。
</p><h1>3 移动惩罚(travel penalty)
</h1><p>该技术主要对具有地理位置信息的item使用。
</p><p>文章分析Foursquare发现用户的移动范围一般都较小，因此在推荐时离用户当前位置较近的地方应该优先推荐，并提出了移动惩罚的方法。
</p><p>文中提出了一种高效的获得离用户当前位置距离最近的k个点的算法。移动惩罚的原理很简单，就是在原有的打分上加上一个惩罚，距离越远，惩罚越大，<img src="http://www.codingart.info/wp-content/uploads/2012/10/102512_0306_LARSALocati2.png" alt=""/>，P(u,i)表示传统的CF打分（可使用全局的，也可以基于用户划分使用），TravelPenalty(u,i)表示u和i的距离。
</p><p>为了避免对所有的推荐候选item都计算上式，我们根据移动惩罚的单调递增顺序计算上式，找到最好的k个item。基本原理是假设下一个item是最高评分，剪掉移动惩罚后和待推荐的第k个评分比较，如果都比最小的(k)评分小，则后面就不可能有更大的了，就不用计算了。
</p><p>同时文章介绍了两种根据移动惩罚增加序列获得item的方法，本文不再介绍。????
</p><h1>4 实验
</h1><p>本文为了评价LARS试用了三个数据集：Foursquare、MovieLens和模拟数据。其实前两个是为了测试推荐的质量，模拟数据是为了测试推荐系统的性能。
</p><h2>4.1 针对不同的金字塔层次的推荐质量
</h2><p>使用Foursquare和MovieLens数据集，比较了传统的协同过滤CF、本文提出的LARS以及只考虑移动惩罚的LARS-T和只考虑用户划分的LARS-U。
</p><p>MovieLens相当于spatial ratings for non-spatial items，Foursquare相当于spatial ratings for spatial items。
</p><p>为评价质量，每个数据集中抽出80%作为测试集，另外的20%作为测试集，而且测试集中都是用户评价较高的item。针对每一个测试机中的评价t，根据评价t的用户和用户位置请求推荐结果，评价标准就是推荐结果中含有t的item的次数。
</p><p>对于Foursquare数据集，测试了四种算法的结果，对于MovieLens，测试了CF和LARS-U，因为movie没有位置信息。
</p><h2>4.2针对不同推荐结果数量的推荐质量
</h2><p><span style="background-color:aqua">针对不同的推荐结果时，其他参数设置为可导致最好的结果的参数</span>。如在Foursquare中金字塔层次最好的是4，则这里设置为4。而在MovieLens中设置为7，因为从上一个实验中获得的结果7最好。
</p><h2>4.3 Storage Vs. Locality
</h2><p>在用户划分的金字塔模型中，需要权衡scalability和locality来动态适应金字塔结构，因此这里分析Storage(scalability的主要表现)和Locality的关系。
</p><p>Scalability和Locality的权衡主要根据权重M，文章根据不同的M设计了实验并分析其结果，发现0.3是一个比较好的结果。
</p><h2>4.4 Scalability
</h2><p>比较了针对不同的评价数量所消耗的存储空间和增加了评价所消耗的重构时间。
</p><h2>4.5推荐请求处理时间
</h2><p>对于Snapshot queries，文章比较了不同的方法所消耗的时间，LARS比如LARS_U好，说明基于提前终止的移动惩罚技术可以获得更快的响应。LARS比LARS-T好说明使用用户分类不仅获得更好的推荐质量，推荐速度也有提高。
</p><p>文章同时还实验说明了Continuous queries的结果。
</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p><p>
?</p>
