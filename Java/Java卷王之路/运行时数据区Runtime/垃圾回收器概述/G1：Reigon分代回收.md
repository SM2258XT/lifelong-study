---
title: G1：Reigon分代回收
updated: 2022-09-16T08:22:08
created: 2022-09-15T19:50:55
---

G1：Reigon分代回收
**并行**、并发、Region、STW
2022年9月15日
19:50

既保证延迟，又保证吞吐量。

**使用Region划分思想来分析垃圾。**
- 它把堆内存分割为很多不相关的区域（Region，物理上不连续的）。使用不同的Region来表示Eden、幸存者0区，幸存者1区，老年代等。
跟踪各个Region里面的垃圾堆积的价值大小（回收所获得的空间大小以及回收所需时间的经验值），在后台维护一个**优先列表**

每次根据允许的收集时间，优先回收价值最大的Region。
- 由于这种方式的侧重点在于回收垃圾最大量的区间，所以我们给G1一个名字：垃圾优先（Garbage First）。
**并行与并发兼备**
并行性：G1在回收期间，可以有多个GC线程同时工作，有效利用多核计算能力。此时用户线程STW
并发性：G1拥有与应用程序交替执行的能力，部分工作可以和应用程序同时执行，因此，一般来说，不会在整个回收阶段发生完全阻塞应用程序的情况
**空间整合**
CMS：“标记-清除”算法、内存碎片、若干次GC后进行一次碎片整理
G1：将内存划分为一个个的region。内存的回收是以region作为基本单位的。Region之间是复制算法，但整体上实际可看作是标记-压缩（Mark-Compact）算法，两种算法都可以避免内存碎片。这种特性有利于程序长时间运行，分配大对象时不会因为无法找到连续内存空间而提前触发下一次GC。尤其是当Java堆非常大的时候，G1的优势更加明显。

**G1回收器的缺点**
1.  相较于CMS，G1还不具备全方位、压倒性优势。比如在用户程序运行过程中，G1无论是为了垃圾收集产生的内存占用（Footprint）还是程序运行时的额外执行负载（overload）都要比CMS要高。
2.  从经验上来说，在小内存应用上CMS的表现大概率会优于G1，而G1在大内存应用上则发挥其优势。平衡点在6-8GB之间。

G1是一款面向服务端应用的垃圾收集器，主要针对配备多核CPU及大容量内存的机器，以极高概率满足GC停顿时间的同时，还兼具高吞吐量的性能特征。
在JDK1.7版本正式启用，移除了Experimental的标识，是JDK9以后的默认垃圾回收器，取代了CMS回收器以及Parallel+Parallel Old组合。被Oracle官方称为“**全功能的垃圾收集器**”。
与此同时，CMS已经在JDK9中被标记为废弃（deprecated）。G1在JDK8中还不是默认的垃圾回收器，需要使用-XX:+UseG1GC来启用。

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)
