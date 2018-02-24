---
title: 使用VimScript读取Python程序中的函数并写入TeX中
tags: 
  - Vim
---
  
<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

### 摘要

 利用VimScript实现读取python文件中的函数并将函数写入TeX文档中，可以在写程序report时提高效率（称为PyToTeX）。具体做法就是读取python文件中定义的函数，置于一个description的环境中，每个函数作为一个item。

### VimScript

VimScript是Vim内部的脚本语言，非常好用，在数据结构，面向对象方面的功能都十分完备，语法较为简单，与python非常相似。具体可以参阅Vim的[user manual]("http://vimcdoc.sourceforge.net/")，在使用其内置函数时，非常建议使用:help (function)，里面介绍十分详细，而且还有着丰富的例子。

### PyToTeX

函数代码如下：

	function PyToTeX(File)
		let _path_ = getcwd() "得到当前文件路径
		let _file_ = readfile(escape(_path_ . '/' . a:File,'\')) "读文件并存入_file_中，存为列表，每行为一个item
		let curline = line('.') "获得TeX文档当前行号
		call setline(curline+1,'\begin{description}')
		let cunt = 2
		for line in _file_
			if match(line,'^def') != -1 "若该行以def开头
				let startloc = matchend(line,'^def\s*') "r.e. 得到其def后第一个非空白符的位置
				let PTT_str = line[startloc :-2] " 去掉冒号
				let PTT_str = substitute(PTT_str,'_','\\_','g') "将函数名中下划线前加上\，以免在TeX文档中造成错误
				call setline(curline+cunt,'\item[' . PTT_str . ']  ' )
				let cunt = cunt + 1
			endif
		endfor
		call setline(curline+cunt,'\end{description}')
		call cursor(curline+2,1) "将光标置于第一个item处
		execute 'normal $'
	endfunction


在VimScript中，为了避免变量被重复申明，可在变量前加入`s:`(source)代表是局部变量，在函数中，需要在函数中使用函数参数时要加上`a:`前缀告诉Vim这是函数参数，不然会出现找不到变量的error，同理，前缀还有`b:`(缓冲区的局部变量)、`w:`(窗口的局部变量)、`g:`(全局变量)、`v:`(Vim预定义的变量)。

在VimScript中，为变量赋值需使用`let var = `语句，使用函数时需使用`call function`语句，循环判断等与Python类似。字符串的连接使用`str1 . str2 `即可。


