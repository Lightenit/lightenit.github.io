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

对于一个元路径$$\mathcal{P}=(A_1,A_2,\cdots,A_l)$$，则对于路径$$P$$计算矩阵$$C_P=W_{A_1,A_2}\cdot W_{A_2,A_3}\cdots W_{A_{l-1},A_l}$$，其中$$W_{A_i,A_j}$$是类型$$A_i$$和$$A_j$$之间的邻接矩阵，对于元图$$M$$，则是分解为元路径$$P_1$$和$$P_2$$，$$C_M=C_{P_1}\odot C_{P_2}$$，$$\odot$$为Hadamard product，
