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

考虑无向图$$G=(V, E), |V|=n, |E|=e$$，邻接矩阵$$A=(a_{ij})\in\mathcal{R}^{n\times n}$$，节点的embedding为$$U\in\mathcal{R}^{n\times m}$$, nonnegative basis $$M\in\mathcal{R}^{n\times m}$$

 ## M-NMF Model


