---
title: G1 Region与四阶段标记
updated: 2022-11-21T16:18:58
created: 2022-11-21T13:37:05
---

G1 Region与四阶段标记
<https://tech.meituan.com/2016/09/23/g1.html>
2022年11月21日
13:37

通过-XX:+UseG1GC参数来启用，作为体验版随着JDK 6u14版本面世，在JDK 7u4版本发行时被正式推出，在JDK 9中，G1被提议设置为默认垃圾收集器

**相较于CMS的出色改进**

G1收集器的设计目标是取代CMS收集器，它同CMS相比，在以下方面表现的更出色：
1.  有**整理内存过程**的垃圾收集器，不会产生很多内存碎片。
2.  STW更可控，在停顿时间上添加了预测机制，用户可以**指定期望停顿时间**。

**G1中几个重要概念**
1.  Region
    - 传统垃圾收集器将连续的内存空间划分为三代，逻辑地址是连续的
G1虽然也分了代，但是**并不连续**，每个代都使用N个不连续、大小相同的Region来表示。单个Region连续。
1.  H代表Humongous，巨大对象，大于单个Region的一半。H将直接分配到OldGen防止反复拷贝移动。
2.  -XX:G1HeapRegionSize可以设定Region大小，必须为2的指数
1.  SATB
看不懂，挖坑
1.  RSet
看不懂，挖坑
1.  Pause Prediction Model
看不懂，挖坑

**GC过程**

G1提供了两种GC模式，YGC和Mixed GC，都是STW的
1.  **YGC**
只选一部分年轻代Region来进行回收。通过控制选择的Region数量，来控制YGC时间开销！
1.  **Mixed GC**
所有年轻代Region + 部分收益高的老年代Region。收益由**Global Concurrent Marking**得出

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)
