---
title: Node Embedding经典方法总结
tags: 
  - Node Embedding
---
 
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

这篇博客主要是总结目前一些经典的Node Embedding的方法，比如LINE, GraRep, DeepWalk, Node2Vec这些。

## LINE (2015 WWW)

LINE考虑了节点中的一阶相似性 (First-order Proximity)和二阶相似性 (Second-order Proximity)，其中一阶相似性是考虑两个节点直接的联系，两个点之间的边权作为两个节点的一阶相似性，二阶相似性是为两个节点的共同邻居节点为媒介计算相似性，计算公式为$$w_{ij}=\sum_{k\in N(i)}{w_{ik}\frac{w_{kj}}{d_k}}$$。

### First-order Proximity

设节点$$i$$的embedding为$$\vec{u_i}$$, 则embedding得到的一阶相似性为：

$$p_1(v_i,v_j)=\frac{1}{1 + exp(-\vec{u_i}^T\cdot\vec{u_j})}$$

之后将分布$$p_1(v_i,v_j)$$与分布$$w_{ij}$$的KL距离尽量小，得到目标函数：

$$O_1=-\sum_{(i,j)\in E}{w_{ij}\log p_1(v_i, v_j)}$$

### Second-order Proximity

由embedding得到的二阶相似性为：

$$p_2(v_j|v_i)=\frac{exp(\vec{u_j'}^T\cdot\vec{u_i})}{\sum_{k=1}^{|V|}{exp(\vec{u'}^T_k\vec{u}_i)}}$$

其中$$\vec{u_j'}$$为节点$$j$$作为context时的向量表示。在文中，作者认为每个节点在作为context时和作为自身节点时的表示是不同的。但是在后面计算时，又将$$\vec{u'_j}$$替换为了$$\vec{u_j}$$，以避免出现平凡解。

此时亦采用KL距离来优化：

$$O_2=-\sum_{(i,j)\in E}{w_{ij}\log p_2(v_j|v_i)}$$

在对$$O_2$$进行优化时，采用了负采样的方法，避免对所有点进行遍历。在实验部分不仅对Social Network和Citation Network做了实验，还对Language Network做了实验。属于实验比较充足，而且有可视化等步骤。

LINE的二阶相似性非常值得借鉴，尤其对于推荐系统中，可以考虑相同的想法。

