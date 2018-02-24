---
title:  "[PaperReading]Style Transfer with Non-Parallel Corpora"
tags: 
  - NLP
  - Style Transfer
---
 
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
 
 这篇文章的想法是提供document D1 in Style S1 和 Style S2 生成document D2 in Style S2. 并且D2与D1的content相同。

 文章的方法分为以下几步：

  1. 考虑一句话中的 n-gram，将其分为(anchor text, variable fragment, anchor text), 若anchor text相同，则认为variable fragment为同义词，构建一些同义词对作为数据库。其间还尝试了利用POS tags，name-entity tag, cosine similarity 对anchor text进行genericize

  2. Language model，文章将n-gram 的概率看作是每个作者的style

  3. 之后对D1中的句子进行同义词替换，利用n-gram 计算所有可能的candidate sentence的概率，取概率最大的sentence作为转换后的句子。

文章进行了较多实验，比如单一corpus的self-transform，parallel corpus的转换，Non-parallel corpus的转换，其中对于Non-parallel的来说，首先找到可替换的fragment 难以处理，原因在于相同anchor text很少，其次替换容易造成意思上改变。总的来说，文章中方法比较naive，只能替换同义词，句式也不会变换，模型无法理解句子的意思，单纯基于一些规则替换，故不能有较好的结果。
