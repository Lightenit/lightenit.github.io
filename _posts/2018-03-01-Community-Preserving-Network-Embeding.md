---
title: "[PaperReading]Community Preseving Network Embedding"
tags: 
  - PaperReading
  - node embedding
---
 
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
 
 这篇文章的主要思想是基于矩阵分解的模型，在矩阵分解的模型上加入了Community的loss。文章的model并不难理解，在推导过程中用Lagrange乘子法对变量逐个更新。

考虑无向图$$G=(V, E), |V|=n, |E|=e$$，邻接矩阵$$A=(a_{ij})\in\mathcal{R}^{n\times n}$$，节点的embedding为$$U\in\mathcal{R}^{n\times m}$$, nonnegative basis $$M\in\mathcal{R}^{n\times m}$$, modularity matrix $$B\in\mathcal{R}^{n\times n}, B_{ij} = A_{ij}-\frac{k_ik_j}{2e}$$, $$k_i$$是节点$$i$$的度, 假设共有$$k$$个community, community indicator $$H\in\mathcal{R}^{n\times k}$$, 则$$tr(H^TH)=n$$

## M-NMF Model

M-NMF模型主要是考虑了community的信息，最大化目标函数$$Q$$

$$Q = tr(H^TBH), s.t. tr(H^TH)=n$$

$$\frac{k_ik_j}{2e}$$表示了节点$$i,j$$之间的边数的期望，当边被随机置放时。

## 节点相似度度量

这里的节点相似度用的是LINE中的一阶相似度(两个节点之间的边权)$$S^{(1)}$$和二阶相似度$$S^{(2)}$$。则相似度矩阵为$$S=S^{(1)}+\eta S^{(2)}$$。则此时的优化函数为：

$$\min \|S-MU^T\|^2_F, s.t. M\geq 0, U\geq 0$$

## 综合考虑

设$$C\in\mathcal{R}^{k\times m}$$，为每个community的embedding，这里假设节点$$i$$属于community $$r$$的概率为$$U_iC_r$$，则综合的优化函数为：

$$\begin{cases}
\min_{M,U,H,C}{\|S-MU^T\|^2_F+\alpha\|H-UC^T\|^2_F-\beta tr(H^TBH)}\\
s.t. M\geq 0, U\geq 0, C\geq 0, tr(H^TH)=n\end{cases}$$

函数优化使用迭代进行优化：

$$
\begin{cases}
M\leftarrow M\odot\frac{SU}{MU^TU}\\
U\leftarrow U\odot\frac{S^TM+\alpha H^TC}{U(M^TM+\alpha C^TC)}\\
C\leftarrow C\odot\frac{H^TU}{CU^TU}\\
H = \arg\min_{H\geq 0}{L(H)}=\alpha \|H-UC^T\|^2_F-\beta tr(H^T(A-B_1)H), s.t. tr(H^TH)=n\\
\end{cases}
$$

$$B_1=(\frac{k_ik_j}{2e})$$，为简化问题，将约束$$tr(H^TH)=n$$改为$$H^TH=I$$

则$$H$$的更新为

$$
H\leftarrow H\odot\sqrt{\frac{-2\beta B_1H+\sqrt{\Delta}}{8\lambda HH^TH}}
$$

其中$$\Delta=2\beta(B_1H)\odot(B_1H)+16\lambda(HH^TH)\odot(2\beta AH+2\alpha UC^T + (4\lambda -2\alpha)H)$$

这篇文章发表于2017AAAI上，整体文章的想法比较直观，但是文章中给出了$$H$$矩阵更新中给出了一些关于上下界的证明（类似EM算法吧），其他的矩阵更新源于其他论文中的公式。期望最近能够将相关公式进行一下推导。在实验中主要与DeepWalk, LINE, GraRep, Node2Vec进行了比较。这里之后可以考虑将community进行推广，每个node可以属于多个community来对node做embedding。
