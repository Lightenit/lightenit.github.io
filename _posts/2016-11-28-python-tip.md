---
title: 使用Python时发现的一个小问题
tags:
  - python
---

### 遇到的问题

最近在用Python写连续Hopfield网络时，发现了numpy中的一个问题，如下所示：

	>>>import numpy as np
	>>>a=np.array([[1,2],[3,4]])
	>>>a[1][:]
	>>>array([3,4])
	>>>a[:][1]
	>>>array([3,4])

这里与直觉差距差距较大，可能也是自己以前MATLAB用多了的原因吧。这里的原因似乎是因为array主要继承了父类列表，所以不能直接取竖列，所以这里`a[1][:]`与`a[:][1]`和`a[1]`的结果都是相同的，都相当于取该列表的第二个元素，即列表`[3,4]`。

### 解决方法

这里问题主要在于写法：

在numpy中的取列操作应该写在同一个中括号中，如下：

	>>>a=np.array([[1,2],[3,4]])
	>>>a[1,:]
	>>>array([3,4])
	>>>a[:,1]
	>>>array([2,4])

即可解决。
