---
title: "机器学习复习2-SVM" 
tags: 
  - SVM
  - 机器学习复习
---
 
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
 
## 支持向量机

### 基本思想

SVM主要用于二分类，模型与感知机类似，其基本想法为利用两个平行的超平面$$wx+b=1$$和$$wx+b=-1$$对数据进行分类，使得$$y_i=+1$$的数据点都落在$$wx+b=1$$的上面，$$y_i=-1$$的点都落下$$wx+b=-1$$的下面，同时使得两个超平面的距离$$\frac{2}{\|w\|}$$尽量大。即

\\[\left\{\begin{align}\min_{w,b}{\frac{1}{2}\|w\|^2} \\
y_i(wx_i+b)\geq 1,\forall i\end{align}\right.}

利用Lagrange Multiper方法有:

\\[f(w,b,u)=\frac{1}{2}\|w\|^2+\sum_{i=1}^{N}{u_i(1-y_i(wx_i+b))}\\]

\\[\begin{align}
\frac{\partial f}{\partial w}=w-\sum_{i=1}^{N}{u_iy_ix_i}=0 \\
\frac{\partial f}{\partial b}=-\sum_{i=1}^{N}{-u_iy_i}=0\end{align}\\]

将上式带入可得到SVM的对偶形式：

\\[\max_{u}{\sum_{i=1}^{N}u_i-\frac{1}{2}{\sum_{i,j=1}^{N}{u_iu_jy_iy_jx_i^Tx_j}}}\\]

约束条件（KKT条件）为

\\[\left\{\begin{align}
u_i\geq 0 \\
\sum_{i=1}^{N}{u_iy_i}=0\end{align}\\]

则$$f(x)=wx+b=\sum_{i=1}^{N}{u_iy_ix_i^T}x+b$$

### SMO(Sequential Minimal Optimization)

SMO算法主要用于解决SVM的对偶形式中$$u$$的最优化选取问题。其步骤如下：

1. 随机取$$u_i,i=1,2\cdots,N$$
2. 任取两个变量（此处尽量选取不满足KKT条件的，会使得模型改进更大）$$u_i,u_j$$，之后固定其他变量，则有$$y_iu_i+y_ju_j=k,(k=-\sum_{l\neq i,j}{u_ly_l})$$
3. 此时原优化问题
