---
title: Python的模块，包概念及结构
layout: post
tags: [Python]
---

> 此篇文章记录关于Python中的模块和包的概念、包的结构、包和模块的引入方法。

-------------------------------

## 引入包和模块

两种语句：

1. import xxx ：导入包或模块
2. from xxx import yyy ( as y ) ：从包中引入其模块或子包/从模块中导入子模块

```
####示例
import package
import module
import package.module
import module.func_module
from package import module
from module import func_module
```

------------------------------

## 模块

一个py文件即为一个模块，文件名即为模块名，将相关代码放入一个模块中有助于组织程序。

模块中不仅可以定义函数和变量，还可以写入可执行代码。

还可以将一个py文件中的一部分视为模块，如一个函数、变量或一段代码。

当模块被导入时，模块代码会被执行且仅仅一次。

***模块内置属性:***

1. \_\_name\_\_ :模块直接执行时，其值为__main__;被其他py文件import时，模块被执行一次，__main_—值为模块名。可用这个属性和分离出模块的测试代码

```
if __name__ == '__main__':
	测试代码
```

2. \_\_all\_\_:用于限定使用from module import * 语句引入的代码

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

***模块补充函数:***

* dir() :返回编译完成字节码文件中所有函数、变量、类名组成的列表。
* sys.path :返回当前环境的PYTHONPATH组成的列表
* imp.reload() :重新加载模块

--------------------------------------

## 包

一个包含有__init__.py文件夹的文件夹即可认为是一个包，包内可以包含模块和子包。

类似于模块，当包被导入时，其__init__.py会被执行且仅仅一次。

有如下包结构

- package1
	- __init__.py
	- module1.py
	- module2.py
- package2
	- __init__.py
	- module3
	- module4

1. 若要`import package1`,则需要在package1的__init__.py文件中加入以下代码

```
from . import module1
from . import module2
```

2. 若要`import package1.module1`或者`from package import module1`,则不会执行package1的__init__.py文件，但是会执行module1一次

3. 若要`form package1 import *`,则会执行__init__.py文件，需要手动指定__all__变量指明包含的模块`__all__ = ['module1','module2']`

4. 若package1的module1模块需要导入package2的module3，则正常情况下，解释器将找不到module3，需要在package1的__init__.py文件中加入语句`sys.path.append("../")`,用于将package1父目录加入解释器的搜索目录

5. 对于嵌套的包结构，如下：

	- package
		- subpackage
			- __init__.py
			- module1
			- module2
	- __init__.py

若`from package.subpackage import module1`，则两个__init__.py文件均会被执行


---------------------------------------

