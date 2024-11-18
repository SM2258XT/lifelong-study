---
title: 构造注入constructor-arg
updated: 2021-08-09T08:08:17
created: 2021-03-07T17:18:22
---

构造注入:spring调用类有参数构造方法，在创建对象的同时，在构造方法中给属性赋值。构造注入使用\<constructor-arg\>标签。

一个\<constructor-arg\>表示构造方法的一个参数。

标签属性：
1.  name：表示构造方法的形参名。
2.  index：表示构造方法的参数的位置，参数从左往右位置是0, 1 ,2的顺序value :构造方法的形参类型是简单类型的，使用value。
3.  ref：构造方法的形参类型是引用类型的，使用ref。

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
