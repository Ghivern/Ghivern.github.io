---
title: 模板类基础
tags: [c++]
layout: post
---

记录关于模板的基本概念和定义形式

一、 模板的意义

模板是c++代码重用的主要特点之一，通过非指定数据类型的类和方法实现；通过在使用模板时实例化之，由编译器实现模板对应实例化的声明。

二、一般格式

```
template <typename T>  //亦可用 template <class T>
class Stack{
	private:
		T a;
	public:
		Stack();
		~Stack() {}
		void setA(const T & ss);
};

template <typename T>
Stack<T>::Stack(const T & ss):a(ss) {}

template <typename T>
void Stack<T>::setA(const T && ss){
	a = ss;
}
```

1. 模板类的定义和实现均放在一个头文件中

2. 模版的实例化形式`Stack<int>`,其中int可以替换为任何一种数据类型，甚至包括模板类(此时可能需要考虑各种运算符和拷贝构造函数的重载,特别是对于数据成员有指针的情况)

三、带有表达式参数的模板

`template <typename T, int n>`

T称为类型参数,n称为表达式参数
	
	* n可以是整形,枚举类型，引用，指针，且不能表达式参数的值，和使用其地址，但是可以使用其值
	* `Stack<int,3> test1;`和`Stack<int,5> test2`将实例化两个模拟类Stack，这是表达式参数的缺点
	* 若成员变量有数组，比如`T a[n];`,则数组占用内存栈，执行速度较快，这也是表达式参数的优点

