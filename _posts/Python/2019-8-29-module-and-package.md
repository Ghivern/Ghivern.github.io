---
title: Python的模块，包概念及结构
layout: post
tags: [Python]
---

>此篇文章记录关于Python中的模块和包的概念、包的结构、包和模块的引入方法。

### 模块

一个py文件即为一个模块，文件名即为模块名，将相关代码放入一个模块中有助于组织程序。

模块中不仅可以定义函数和变量，还可以写入可执行代码。

还可以将一个py文件中的一部分视为模块，如一个函数、变量或一段代码。

当模块被导入时，模块代码会被执行且仅仅一次。

	模块内置属性：

		* __name__ :模块直接执行时，其值为__main__;被其他py文件import时，模块被执行一次，__main__值为模块名。可用这个属性和分离出模块的测试代码
		```
		if __name__ == '__main__':
			测试代码
		```

		* __all__:用于限定使用from module import * 语句引入的代码
		```
		# module.py
		__all__ = ['f1']

		def f1():
		 	return 0

		def f2():
			return 1
		```

		这样在test.py文件中
		```
		from module import *
		f1() #正确执行
		f2() #不能执行
		```

	模块补充函数：

		* dir() :返回编译完成字节码文件中所有函数、变量、类名组成的列表。
		* sys.path :返回当前环境的PYTHONPATH组成的列表
		* imp.reload() :重新加载模块

### 包

一个包含有__init__.py文件夹的文件夹即可认为是一个包，包内可以包含模块和子包。

类似于模块，当包被导入时，其__init__.py会被执行且仅仅一次。

### 引入包和模块

两种语句：

* import xxx ：导入包或模块
* from xxx import yyy ( as y ) ：从包中引入其模块或子包/从模块中导入子模块

```
####示例
import package
import module
import package.module
import module.func
from package import module
from module import func
```

