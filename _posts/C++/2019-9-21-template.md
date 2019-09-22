---
title: 模板类基础
tags: [C++]
---

> 记录关于模板的基本概念和定义形式

## 一、 模板的意义

模板是c++代码重用的主要特点之一，通过非指定数据类型的类和方法实现；通过在使用模板时实例化之，由编译器生成对应类的声明。

---------------

## 二、一般格式

```c++
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

---------------

## 三、带有表达式参数的模板

`template <typename T, int n>`

T称为类型参数,n称为表达式参数
	
* a. n可以是整形,枚举类型，引用，指针，且不能修改表达式参数的值，和使用其地址，但是可以使用其值

* b. `Stack<int,3> test1;`和`Stack<int,5> test2`将实例化两个模拟类Stack，这是表达式参数的缺点

* c. 若成员变量有数组，比如`T a[n];`,则数组占用内存栈，执行速度较快，这也是表达式参数的优点

---------------

## 四、模板的多功能性

模板可以使用常规类的技术；模板类可以作为基类，组件类，还可以作为类型参数

```c++
template <typename T>
class Array(){
	private:
		T entry;
};

template <typename Type>
class ArrayPlus:public Array<Type> {...};  //模板类作为基类

template <typename Tp>
class Stack{
	Array<Tp> ar;    //模板类作为组件类
};

Array< Stack<int> > a;  //模板类作为类型参数
						//要将两个>符号至少间隔一个空格，以区别>>运算符
```

* a. 递归使用使用模板

```c++
tempalte <typename T,int n>
class Array{
	private:
		T a[n];
	public:
		T opeartor[](int i) const{
			return a[i];
		}
};

//实例化
Array< Array<int,5>, 10> test; //这种情况下，其等价效果为int test[5][10];
```

* b. 使用多个参数

```c++
template <typename T1,typename T2>
class Pair{...};
```

* c. 默认类型参数

```c++
template <typename T1,typename T2 = int>
class Test{...};
//不能为函数模板提供默认类型参数，然而可为非类型参数提供默认值
```

---------------

## 五、模板的具体化

1.隐式实例化：创建对象时，指出参数类型，编译器根据通用模板生成具体类定义

2.显式实例化：即使不创建对象，编译器也将根据通用类模板生成类声明
	
* a. 格式

```c++
	template class ClassName<string,100>;
```

* b. 显示实例化的声明必须放在模板定义所在名称空间(后面两种具体化也是如此)

<br>

3.显式具体化:对特定类型的定义，对模板进行修改，使其行为不同

```C++
template <>
class Classname<specialized-type-name>{
	...
};
```

<br>

4.部分具体化：可以给一部分类型参数指定具体类型；template后面的<>中声明没有被具体化的类型参数

```C++
template <typename T1,typename T2>
class Test{
	---
};

//部分具体化T2
template <typename T1> class Test<T1,int>{
	...
};
```

* a. 为指针提供具体化模板;若没有进行部分具体化，T将转换为char * 类型

```C++
template<typename T>
class Feed{
	...
};
              
template<typename T*>
class Feed{
	...             
};
      
Feed<char> fb1;
Feed<char *> fb2;

```

* b. 若有多个模板可供使用，编译器将使用具体化程度高的模板

* c. 部分具体化一种用法

```C++
template <Typename T1,Typename T2>
class Test{
	...
};

template <Typename T1>
class Test<T1,*T1>{
	...
};

Test<int,int *> t;
//将使用下面一个部分具体化模板生成类的定义
```

----------------

## 六、成员模板

模板可以作为结构，类，或者模板类的成员

```c++
using std::cout;
using std::endl;
template <typename T>
class A{
private:

	template <typename V>
	class B{
	private:
		V val;
	public:
		B(V v = 0):val(v) {}
		V value() const {return val;}
		void show() {cout<<val<<endl;}
	};

	B<T> q;
	B<int> n;

public:
	A(T t,int i):q(t),n(i) {}
	template <typename U>
	U calculate(U u,T t) {return (q.value+n.value)*u/t;}
	void show() {q.show();n.show();}
};
//新的C++标准支持在类模板中什么，声明类模板和函数模板，而将实现外面实现他们(可参考C++ Primer Plus p585)

int main(){
	A<double> test(3.5,3);  //此时test.q的类型是B<double>
	 						// test.n的类型是B<int>
	cout<<test.calculate(3,3)<<endl; //此时U被设为int
	test.calculate(3.5,3)<<endl; //此时U被设为double
	return 0;
} 
```

## 七、将模板用作参数(模板参数)

1.模板除了可以包含类型参数和非类型参数，还可以包含本身就是模板的参数

2.`template <template <typename T> class Thing>`

* a.其中`template <typename T> class`是类型，Thing是参数

* b.如有class A{};使用的是使用的是上述模板参数,则有

```c++
template <template <typename T> class Thing>
class A{
	private:
		Thing<int> a;
		Thing<char> b;
};

template <typename V>
class B{

};


A<B> test;  /*实例化test的私有数据成员a,b时，分别用B<int>和B<char>
				替换Thind<int>和Thing<char>
			*/

```

* c.可以混合使用模板参数的常规参数，如
```c++
template <template <typename T> class Thing,typename U,typename V>
class A{
private:
	Thing<U> s1;
	Thing<V> s2;
	...
};

template <typename T>
class B{
	...
};
//此时A的声明形式
// A<B,int,char> test;
```

## 模板别名(C++11)

1.老的声明别名方式

```c++
typedef std::array<double,12> arrd;
typedef std::array<int,12> arri;
typedef std::array<std::string,12> arrs;
arrd d;
arri i;
arrs s;
```

2.C++新增功能，可以使用模板提供一系列别名

```c++
template <typename T>
	using arrtype = std::array<T,12>;
arrtype<double> a;  //等价于array<double,12> a;
arrtype<int> b;  //等价于array<int,12> b;
				 //arrtype<T>表示std::array<T,12>
```

3.using=可用于非模板,这种语法于常规typedef等价

```c++
typedef const char * pc1;
using pc2 = const char *;
typedef const int *(*pa1)[10];
using pa2 = const int *(*)[10];
```
