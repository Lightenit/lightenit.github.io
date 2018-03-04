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

## GraRep (2015 CIKM)

GraRep也是基于矩阵分解的方法，同时希望能够考虑到$$k$$步的相似性。这里的$$k$$是考虑前$$K$$步的相似性的意思。这里主要考虑是Random Path的相似性，节点$$i$$和$$j$$的相似性相当于节点$$i$$经过$$k$$步走到节点$$j$$的概率。

首先固定步数$$k$$，此时的相似度为$$A^k$$. $$S$$为邻接矩阵。

$$
\begin{cases}
A = D^{-1}S\\
D = diag\{\sum_p{S_{ip}}\}
$$

则此时loss function为：

$$
\begin{aligned}
L_k(w) = (\sum_{c\in V}p_k(c|w)\log\sigma(\vec{w}\cdot\vec{c}))+\lambda\mathcal{E}_{c'\sim p_k(V)}{\log\sigma(-\vec{w}\cdot\vec{c'})}\\
A_{w,c}^k\cdot\log\sigma(\vec{w}\vec{c})+\frac{\lambda}{N}\sum_{w'}{A_{w',c}^k\cdot\log\sigma(-\vec{w}\vec{c})}
\end{aligned}
$$

设$$e=\vec{w}\cdot\vec{c}$$, $$\frac{\partial{L_k}}{\partial{e}}=0$$,有

$$\vec{w}\vec{c}=\log(\frac{A_{w,c}^k}{\sum_{w'}{A^k_{w',c}}})-\log\beta, \beta=\frac{\lambda}{N}$$

这里和LINE类似吧，也是两个矩阵，$$W$$代表节点本身的表示，而$$C$$代表每个节点作为context时的表示。之后通过SVD分解得到$$W$$:

$$
\begin{cases}
Y_{i,j}^k=W_i^k\cdot C_j^k=\log(\frac{A_{i,j}^k}{\sum_t{A_{t,j}^k}})-\log\beta\\
X_{i,j}^k=\max(Y_{i,j}^k,0)\\
X^k = U^k\Sigma^k(V^k)^T\\
W^k = U_d^k(\Sigma_d^k)^{\frac{1}{2}}\\
\end{cases}
$$

$$U_d^k$$为矩阵$$U^k$$的前$$d$$列。由此可以得到在$$k$$步时的表示。则GraRep算法流程为：

1. 计算出$$A^k, k=1,2,\cdots,K$$
2. 对每个$$k$$, 计算出每个节点的表示$$W^k$$
3. 将所有的$$W^k$$直接拼接，得到最终的表示$$W$$
