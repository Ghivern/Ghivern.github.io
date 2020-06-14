---
title: 集合初识
tags: [C#]
---

> 集合基本概念

Collections {#sec-1}
===========

数组 {#sec-1-1}
----

-   C Sharp中的数组其实是集合类中的一种

-   实现了IEnumerable$\backslash$IList接口,但是不支持IList的高级功能,其表示大小固定的列表项

-   System.Collections命名空间中的类ArrayList也实现了IEnumerable$\backslash$IList接口,实现方式比数组复杂,列表项目大小可变

集合类概念 {#sec-1-2}
----------

### 描述 {#sec-1-2-1}

-   用于处理对象列表

-   功能比数组强大

-   功能大多是通过实现System.Collections命名空间中的接口而获得的（说明集合已经标准化了）

-   可通过接口定制化集合，实现强类型化的集合

### System.Collections命名空间中的基本接口 {#sec-1-2-2}

-   IEnumerable 提供可以迭代集合中的项的功能

-   ICollection
    继承于IEnumerable,提供获取集合中项的个数，并能将项复制到一个简单数组中的功能

-   IList
    继承于IEnumerable和ICollection,提供了集合的列表项，允许访问这些项，并提供其他一些与项列表相关的一些功能

-   IDictionary
    继承于IEnumerable和ICollection,类似于IList,但是提供了可以通过键值访问的项列表

-   System.Collections命名空间中还包含


