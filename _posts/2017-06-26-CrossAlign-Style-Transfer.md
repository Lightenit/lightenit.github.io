---
title: "[PaperReading]Style Transfer from Non-Parallel Text by Cross-Alignment"
tags: 
  - Style Transfer
  - NLP
---
 
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
 
 这篇文章总体上来看也是通过VAE来处理Style Transfer的问题，并且在模型中加入了对抗的部分。具体模型如下：

 对于database中的sentence $$x$$，认为其有两个隐变量影响，分别是$$z$$(content)和$$y$$(style), 由于目前已知为条件变量$$p(x\|y)$$，文中的模型主要由E(encoder), G(generator), D(discriminator)组成。其中：

 E: infer content z from sen x and style y.(input: x and y, output: z)

 G: generator sen x from style y and content z. (input: y and z, output: x)

 D: distinguish sen from two database. 

 文中的E,D使用RNN实现，D使用了两个模型，第一个alignment model就用了一个隐层的前馈nn实现，第二个cross-alignment model中使用了两个D，$$D_1$$主要负责区分database 1中的句子，$$D_2$$主要负责区分database 2中的句子，也就是因此叫做cross吧。

 文章中的style一直作为隐变量存在，但是language 主要以bi-gram来表示。这里其实有一定的讨论空间，bi-gram模型表示中蕴含了哪些内容，是否含有content？在我看来更多的是style的信息。那么如何运用好n-gram的信息值得深入研究。
