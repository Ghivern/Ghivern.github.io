---
title: 创建自己的python包
tags: [Python]
---

>今天学会了如何实现一个基本的python包，还有许多地方不是很理解，后面学习后会拓展

**第一步**

创建一个文件夹，文件夹的名字即为包的名字(就是import packname中的packname)，在这里举例命名为animals

**第二步**

在animal文件夹下创建两个py文件，里面写自己的功能函数，下面两个示例，分别是dogs.py和birds.py
```python
# birds.py

def say():
	print("I am a bird.")
```
```
# dogs.py

def say():
	print("I am a dog.")
```

**第三步**

在animal文件夹下新建一个文件`__init__.py`,这个文件是必须的，目前我知道的用途是标识此文件夹是“包文件夹”，内容用于说明包含关系(目前我理解的说法，应该不准确)，内容如下：
```
from . import dogs
from . import birds
```

**第四步**

测试自己的animal包

在animal所在文件夹下新建test.py,内容如下：
```
import animal as a
a.dogs.say()
a.birds.say()
```

在animal所在文件夹下执行python.exe test.py(或者将animal包所在路径加入环境变量，即可在任何路径下引用此包)
