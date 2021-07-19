---
title: "一些常用的命令和快捷键"
tags: 
  - Linux
---
 
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
 
这篇主要用来记录平常会用到的一些快捷键和命令，每学到一个就记在这里避免忘记。

### VIM

  1. 宏，录制过程： `qa`(normal mode) + 需要反复执行的命令 + `q`(normal mode). 这里`qa`是指宏的名字为`a`，也可以用其他的。在调用宏的时用`@a`即可。`n@a`表示调用该宏 n 次。之前的应用场景是在写tex的时候将一列数中加入 &, 变成表格。使用`f<Tab>i&`即可。值得注意的是在多次调用的时候记得要宏的最后换到下一行并把光标设到句首。


### Linux Terminal

  1.  `Ctrl + L` 在终端中清屏，效果类似Matlab中 `clc`，但该命令应该是将当前的命令行移到屏幕最上方，向上滑动还是可以看到之前的命令。对于我这种轻度强迫症很适用。
