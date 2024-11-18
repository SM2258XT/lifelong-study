---
title: GC分类和触发机制
updated: 2022-05-13T20:17:08
created: 2022-05-06T09:53:01
---

GC分类和触发机制
2022年5月6日
9:53

1.  我们都知道，JVM的调优的一个环节，也就是垃圾收集，我们需要尽量的避免垃圾回收，因为在垃圾回收的过程中，容易出现STW（Stop the World）的问题，**而Major GC和Full GC出现STW的时间，是Minor GC的10倍以上**
2.  JVM在进行GC时，并非每次都对上面三个内存区域一起回收的，大部分时候回收的都是指新生代。针对Hotspot VM的实现，它里面的GC按照回收区域又分为两大种类型：一种是部分收集（Partial GC），一种是整堆收集（FullGC）
    - 部分收集：不是完整收集整个Java堆的垃圾收集。其中又分为：
      - 新生代收集（Minor GC/Young GC）：只是新生代（Eden，s0，s1）的垃圾收集
      - 老年代收集（Major GC/Old GC）：只是老年代的圾收集。
      - 目前，只有CMS GC会有单独收集老年代的行为。
    - 整堆收集（Full GC）：收集整个heap和method area（或meta space）的垃圾收集。
    - 混合收集（Mixed GC）：收集整个新生代以及部分老年代的垃圾收集。目前，只有G1 GC会有这种行为
注意，很多时候Major GC会和Full GC混淆使用，需要具体分辨是老年代回收还是整堆回收。

Minor GC/Young GC触发机制
1.  当年轻代空间不足时，就会触发Minor GC，这里的年轻代满指的是**Eden满**。Survivor满不会主动引发GC（我的理解：因为s区根本就不会满！溢出的都直接放到Old去了）
在Eden区满的时候，会**顺带触发**s0区的GC，也就是被动触发GC（每次Minor GC会清理年轻代的内存）
1.  因为Java对象大多都具备朝生夕灭的特性，所以Minor GC非常频繁，一般回收速度也比较快。这一定义既清晰又易于理解。
2.  Minor GC会**引发STW**（Stop The World），暂停其它用户的线程，等垃圾回收结束，用户线程才恢复运行

Major GC触发机制
1.  指发生在老年代的GC，对象从老年代消失时，我们说 “Major Gc” 或 “Full GC” 发生了
2.  出现了MajorGC，经常会伴随至少一次的Minor GC。（但非绝对的，在Parallel Scavenge收集器的收集策略里就有直接进行MajorGC的策略选择过程）
也就是在老年代空间不足时，会先尝试触发Minor GC（哈？我有点迷？），如果之后空间还不足，则触发Major GC
1.  Major GC的速度一般会比Minor GC**慢10倍以上**，STW的时间更长。
2.  如果Major GC后，内存还不足，就报OOM了

Full GC触发机制（后面细讲）
1.  调用System.gc()时，系统建议执行FullGC，但是不必然执行
2.  老年代空间不足
3.  方法区空间不足
4.  通过Minor GC后进入老年代的平均大小大于老年代的可用内存（其实就是老年代空间不足，也就是说Minor GC可能会引起Full GC？）
5.  由Eden区、survivor space0（From Space）区向survivor space1（To Space）区复制时，对象大小大于To Space可用内存，则把该对象转存到老年代，且老年代的可用内存小于该对象大小

说明：Full GC 是开发或调优中尽量要避免的。这样STW时间会短一些

