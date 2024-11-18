---
title: Hashmap 升级历程概览
updated: 2022-11-21T19:54:34
created: 2022-05-16T18:14:28
---

Hashmap 升级历程概览
<https://tech.meituan.com/2016/06/24/java-hashmap.html>
2022年5月16日
18:14

1.  底层数据结构从“数组+链表”改成“数组+链表+红黑树”，主要是优化了 hash 冲突较严重时，链表过长的查找性能：O(n) -\> O(logn)。
2.  计算 table 初始容量的方式发生了改变，老的方式是从1开始不断向左进行移位运算，直到找到大于等于入参容量的值；新的方式则是通过“5个移位+或等于运算”来计算。
3.  优化了 hash 值的计算方式，老的一顿瞎JB操作，新的只是简单的让高16位参与位运算。
4.  扩容时插入方式从**“头插法”改成“尾插法”**，避免了并发下的死循环。
5.  扩容时计算节点在新表的索引位置方式从“h & (length-1)”改成“hash & oldCap”，性能可能提升不大，但设计更巧妙、更优雅。

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)
