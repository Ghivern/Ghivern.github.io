---
title: Python的模块，包概念及结构
tags: [Python]
---

此篇文章记录关于Python中的模块和包的概念、包的结构、包和模块的引入方法.

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

*模块内置属性:*

1. \_\_name\_\_ :模块直接执行时，其值为__main__;被其他py文件import时，模块被执行一次，__main_—值为模块名。可用这个属性和分离出模块的测试代码
```
if __name__ == '__main__':
	测试代码
```
2. \_\_all\_\_:用于限定使用from module import * 语句引入的代码
```
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

*模块补充函数:*

+ dir() :返回编译完成字节码文件中所有函数、变量、类名组成的列表。
+ sys.path :返回当前环境的PYTHONPATH组成的列表
+ imp.reload() :重新加载模块

--------------------------------------

## 包

一个包含有__init__.py文件夹的文件夹即可认为是一个包，包内可以包含模块和子包。

类似于模块，当包被导入时，其__init__.py会被执行且仅仅一次。

有如下包结构

- package1
	- \_\_init\_\_.py
	- module1.py
	- module2.py
- package2
	- \_\_init\_\_.py
	- module3
	- module4

1. 若要`import package1`,则可要在package1的__init__.py文件中加入以下代码,否则包内模块不会被执行
```
from . import module1
from . import module2
```
此时package1在环境的名称空间内,module1和module2在package1的名称空间内

2. 若要`form package1 import *`,则会执行__init__.py文件，需要手动指定__all__变量指明包含的模块`__all__ = ['module1','module2']`,此时列表中内容在环境的名称空间内

3. 若package1的module1模块需要导入package2的module3，则正常情况下，解释器将找不到module3，需要在package1的__init__.py文件中加入语句`sys.path.append("../")`,用于将package1父目录加入解释器的搜索目录

4. 对于嵌套的包结构，如下：
	- package
		- subpackage
			- \_\_init\_\_.py
			- module1
			- module2
	- \_\_init\_\_.py
	- 若`from package.subpackage import module1`，则两个__init__.py文件均会被执行


---------------------------------------

## 一些说明

包结构中比较复杂的是包的__init__.py文件，我对这个文件功能的理解如下：

1. 标识一个包

2. 当import某个包时，首先执行__init__.py文件，通过这个文件导入包中的模块或子包

3. 在只和模块在一个包中时
	- (1)其应包含import模块的语句,引入模块；为了更方便，比如requests库，我们`import requests`后，不用通过requests.api.get()使用get方法，只需要requests.get()即可，查看requests包源码可发现，其在__init__.py文件中有语句`from .api import get`,这样get这个名称就可以在包的名称空间中使用；而且此时api.py文件会被执行，也就是说api.py模块会被导入，导入的不仅仅是get方法。
	- (2)若不使用上述引入语句，而是将包中的模块加入__all__列表,则通过`from package import *`时没有问题，但是通过`import package`引入包时，模块是不会被执行的
	- (3)其实__init__.py文件中也可以写执行代码，我看了一些第三方包，他们的有些方法就实现在这个文件中。
	- (4)综上，使用(1)所叙述是合理的，而且有些方法可以直接写在__init__.py文件中。

4. 一个包中既有子包又有模块时
	- (1)对于模块和上述3一样
	- (2)对于子包
		+ 在package的__init__.py文件中执行`from .subPackage import func`语句后会执行子包的__init__.py文件,并将func方法名放入package名称空间;也可通过`from . import subPackage`语句引入子包,并会执行子包的__init__.py,此时将子包名放入Package的名称空间

5. \_\_all__列表,我的理解是当使用`from package import *`时导入,将列表中的内容加入package的名称空间,甚至子包模块的内容也可以加入此列表

