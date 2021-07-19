---
title: "[PaperReading]Controlling Linguistic Style Aspects in Neural Language Generation"
tags: 
  - NLP
  - Style Transfer
---
 
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

这篇文章的作者之一是Yoav Goldberg，不过文章的模型比较简单。主要就是建立在RNN上，再人工的加上几个style和content特征加在条件概率上，其实相当于人工的为数据集进行了一个分组，分组之后分别训练，这样很显然会对ppl有提升。

文中的style取了4个特征，content取了2个特征，分别如下：

content: sentiment score, topic.

style: the length of the text, whether it is descriptive, whether it is written in professional style, whether it is written in a personal voice. 

这六个特征都是通过一些启发性特征来判断标记的，这里就不做介绍。其中每个特征通过人工设成离散取值，就可以使用向量表示所有的特征，然后直接concat起来为$$c$$。

之后直接利用公式：

\\[ P(\omega_1, \omega_2, \cdots, \omega_n)=\prod_{t=1}^{n}{P( \omega_t\|\omega_1,\cdots,\omega_{t-1}, c)} \\]

直接带入数据集进行训练，在利用训练好的进行生成即可。

文章中的评价方法是用ppl计算，从上分析可以看出ppl的提升是必然的。另外将生成的句子利用上面的启发式方法判断，观察生成的句子是否满足想要的特征，正确率大都是百分之90+。效果还不错，不过感觉启发意义并不大。
