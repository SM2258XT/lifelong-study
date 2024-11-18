---
title: StringTable：字符串常量池
updated: 2022-05-13T20:17:08
created: 2022-05-07T09:57:38
---

StringTable：字符串常量池
2022年5月7日
9:57

在Java语言中有8种基本数据类型和一种比较特殊的类型String。这些类型为了使它们在运行过程中速度更快、更节省内存，都提供了一种常量池的概念。
**private final** char value\[\]; 具有不可变性。字符串值声明在**字符串常量池**中。
String在jdk8及以前内部定义了final char value\[\]用于存储字符串数据。JDK9时改为byte\[\]。byte\[\]加上编码标记，节约了一些空间。同时StringBuffer和StringBuilder也同样做了修改。

1.  字符串常量池**不会存储相同内容**的字符串
底层是一个固定大小的Hashtable，默认值大小长度是60013（JDK8）。如果放进String Pool的String非常多 ，就会造成Hash冲突严重，从而导致链表会很长，而链表长了后直接会造成的影响就是当调用String.intern()方法时性能会大幅下降。使用-XX:StringTablesize可设置StringTable的长度

当对字符串重新赋值时，需要**重新指定内存区域**赋值，不能使用原有的value进行赋值

String s1 = "abc"; //字面量定义的方式，"abc"存储在字符串常量池中，使用的是符号引用： \#2

String s2 = "abc"; //使用的是**同一个**符号引用： \#3

System.out.println(s1 == s2); //判断地址：true
1.  字符串连接
连接的都是字面量

在编译阶段就会直接转换为新的字面量

String s1 = "aaa" + "bbb" // 编译后直接为：String s1 = "aaabbb"
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
