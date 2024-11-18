---
title: Serial、Serial Old
updated: 2022-09-16T10:42:25
created: 2022-09-15T15:44:02
---

**早已弃用**

Serial回收新生代、Serial Old回收老年代

**串行收集、STW**
这个收集器是一个单线程的收集器，它只会使用一个CPU（串行）或一条收集线程去完成垃圾收集工作。更重要的是在它进行垃圾收集时，必须STW

**优势**
1.  单CPU环境性能好
单CPU无需线程切换开销。
1.  内存小时性能好。
小型桌面应用内存小，Serial可以短时间完成扫描，快速完成垃圾回收。

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
