---
title: StringTable放在哪
updated: 2023-02-26T10:10:18
created: 2022-05-13T18:53:02
---

字符串常量池在Java内存区域的哪个位置？
在JDK6.0及之前版本，字符串常量池是放在Perm Gen区(也就是方法区)中；
在JDK7.0版本，字符串常量池被移到了堆中了。至于为什么移到堆内，大概是由于**方法区的内存空间太小了**
并且String**可以直接引用Heap**，无需每次都拷贝一份副本到StringTable了！
（堆内是可以进行回收的，然后方法区也是能回收的，但是本身区域内存比较少，如果用的字符串常量太多了，也会抛java.lang.OutOfMemoryError:PermGenspace 异常）
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
