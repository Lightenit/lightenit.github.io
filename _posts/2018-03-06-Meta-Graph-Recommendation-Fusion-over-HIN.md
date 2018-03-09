---
title: "[PaperReading]Meta-Graph Based Recommendation Fusion over Heterogeneous Information Networks"
tags: 
  - HIN
  - Recommendation System
---
 
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
 
这篇文章发表在2017 KDD上，是基于HIN(针对User-Item类型)的推荐系统，文章的思路主要可以分成两部分，第一部分是通过Meta-Graph得到每个节点的embedding，这里的embedding是针对于不同Meta-Graph得到不同embedding。第二部分是基于之前的embedding作为特征输入Factor Machine进行预测。得到Rating。具体细节如下：

## Meta-Graph based embedding

对于一个元路径$$\mathcal{P}=(A_1,A_2,\cdots,A_l)$$，则对于路径$$P$$计算矩阵$$C_P=W_{A_1,A_2}\cdot W_{A_2,A_3}\cdots W_{A_{l-1},A_l}$$，其中$$W_{A_i,A_j}$$是类型$$A_i$$和$$A_j$$之间的邻接矩阵，对于元图$$M$$，则是分解为元路径$$P_1$$和$$P_2$$，$$C_M=C_{P_1}\odot C_{P_2}$$，$$\odot$$为Hadamard product。由此对于$$L$$个元图，可以得到$$L$$个相似度矩阵$$R^1,\cdots, R^L$$. 则优化一下函数得到User和Item的embedding：

$$
\min_{U,B}\frac{1}{2}\|P_{\Omega}(UB^T-R)\|^2_F+\frac{\lambda_u}{2}\|U\|_F^2+\frac{\lambda_b}{2}\|B\|^2_F
$$

其中$$[P_{\Omega}(X)]_{ij}=X_{ij}$$ if $$\Omega_{ij}=1$$ and 0 otherwise. 得到User和Item的embedding：$$U^{(1)},B^{(1)},\cdots,U^{(L)},B^{(L)}$$.

在得到embedding之后，对于User $$u_i$$和Item $$b_j$$, 得到特征：

$$x_n=[u_i^{(1)},\cdots,u_i^{(l)},\cdots,u_i^{(L)},b_j^{(1)},\cdots,b_j^{(l)},\cdots,b_j^{(L)}]$$

则对于不同的$$u_i, b_j$$训练因子机模型：

$$
y^n(w,V)=w_0+\sum_{i=1}^d{w_ix_i^n}+\sum_{i=1}^d\sum_{j=i+1}^d{<v_i,v_j>x_i^nx_j^n}$$

$$\min_{w,V}\sum_{n=1}^N{(y^n-y^n(w,V))^2}$$

之后，对于因子机模型加入了正则化（lasso）：

$$
h(w,V)=\sum_{n=1}^N{(y^n-y^n(w,V))^2} + \lambda_w\sum_{l=1}^{2L}{\|w_l\|_2} + \lambda_v\sum_{l=1}^{2L}{\|V_l\|_F}$$

之后模型在Amazon，Douban和Yelp数据集上进行了实验。结果提升了蛮多的。这里因子机的优化使用了mmAPG算法。这个模型有些像是两个模型的结合，比较简单一点吧。可以借鉴一下。
