---
title: JVM空间大小调优
updated: 2022-11-22T07:53:02
created: 2022-11-21T14:44:35
---

**参数基本策略**
各分区的大小，对GC影响很大，如何将其调整至合适大小，分析活跃数据大小是很好的切入点。
活跃数据大小：Full GC之后，老年代空间平均大小。（可以通过GC日志中Full GC之后老年代数据大小得出）

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
