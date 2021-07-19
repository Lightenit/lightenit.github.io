---
title: "Vim针对于Python的配置"
tags:
  - Vim
---

### 需要解决的问题

在平时写Python文件时，在文件头总会要加上这两行代码：

	#! /usr/bin/python
	# -*- coding:utf-8 -*-

由此想来改写Vim的配置，使得以后写Python文件时，能够自动的生成这两行代码，省去很多麻烦。

### 解决方法

要做到以上事情需要清楚以下几点：

* 找到配置文件.vimrc，一般直接在根目录下自己vi一个就可以

* 配置的方法，配置方法主要参考了[vim的用户手册](http://vimcdoc.sourceforge.net/)，这里主要介绍几个自己这次用到的一些，以后用到其他的时会逐渐添加：

这里主要用到的是`autocmd`,`autocmd`的标准格式如下：

	:autocmd [group] {events} {file_pattern} [nested] {command}

  1. `[group]` 该选项主要为自动命令分组执行命令，比如对C语言一类的程序分成一组，然后再对Python/Matlab一类的程序分为一组，可以直接对一组进行操作，比如删除，实例如下：

	:augroup cprograms
	:  autocmd BufReadPost *.c,*.h :set sw=4 sts=4
	:  autocmd BufReadPost *.cpp   :set sw=3 sts=3
	:augroup END

  2. `{events}` 事件，常用的事件有`BufReadPost`，在一个文件被调入编辑时触发；`Filetype`，在Vim能够正确的识别文件类型的时候触发，`BufNewFile`，在开始编辑一个新文件时触发

  3. `{file_pattern}` 文件类型，如`*.c`，`*,py`等

  4. `{command}` 要执行的命令

 最后放上目前自己的Vim配置文件：

	set nu
	syntax on
	filetype plugin on
	set si
	set relativenumber
	set nocompatible
	autocmd BufNewFile *.py exec ":call SetTitle()"
	function SetTitle()
		call setline(1,"#! /usr/bin/python")
		call setline(2,"# -*- coding:utf-8 -*-")
		call setline(3,"")
	endfunction
	autocmd BufNewFile * normal G

分别设置了显示行号，文本高亮，智能缩进，相对行号，与旧版vi不兼容和对py文件自动加入文件头。最后说一句：懒惰即美德。
