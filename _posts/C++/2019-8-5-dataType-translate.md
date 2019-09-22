---
title: C++ - 数据类型转换
layout: post
tags: [C++]
---

记录关于C++数据类型转换的内容



基本数据类型：
整形：signed char,char,unsigned char
          short,unsigned short
          int,unsigned
          long,unsigned long
          long long,unsigned long long
浮点型：float
              double,long double
布尔型：bool
          
c++进行类型转换的情况
1、将一种算数类型的值赋给另一种算数类型的变量时，c++将对值进行转换
2、表达式中包含不同的类型时，c++将对值进行转换
3、将参数传递给函数时，c++将对值进行转换

具体内容：
1、初始化和赋值进行转换：值将被转换为接受变量的类型(将级别高的值转换为级别低的类型会出现数据错误)

2、以{}方式初始化时进行转换：初始化列表不允许缩窄（narrowing），否则编译时   
     C++编译器将会报错
     
3、表达式中的转换：C++编译器根据其检验表进行自动转换
    校验表：
     (1)如果有一个操作数类型是long double,则将另一个操作数转换为long double
     (2)否则，如果有一个操作数的类型是double,另一个操作数转换为double
     (3)否则，如果一个操作数类型是float，另一个操作数转换为float
     (4)否则，说明操作数均为整形，执行整体提升
     (5)在这种情况下，如果两个操作数都是有符号或无符号，且其中一个操作数的级别比另一个低，则转换为较高级别的类型
     (6)如果一个操作数是有符号的，另一个操作数是无符号的，且无符号操作数级别比有符号操作数级别高，则将有符号操作数转换为无符号操作数所属的类型
     (7)否则，如果有符号类型可以表示无符号类型的所有可能取值，则将无符号操作数转换为有符号操作数所属类型
     (8)否则，将两个操作数均转换为有符号类型的无符号版本
     上述的整形级别如下：
     整形级别（由低向高）：signed char,short,int,long,long long.
     类型char,signed char,unsigned char的级别相同
     类型bool的级别最低
     wchar_t,char16_t,char32_t的级别与其底层类型相同
     
4、传递参数时转换：类型转换由函数原型控制（可取消原型对参数传递的控制）

5、强制类型转换：显示的将类型进行强制转换
         格式：(typeName)value   //old c syntax
                    typeName(value)  //new c syntax


参操文献
[1].C++ Primer Plus 中文版，Stephen prata，2012年7月第11版