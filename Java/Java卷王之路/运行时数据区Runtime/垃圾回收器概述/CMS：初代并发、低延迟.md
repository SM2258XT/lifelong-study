---
title: CMS：初代并发、低延迟
updated: 2022-09-16T08:24:06
created: 2022-09-15T19:03:54
---

CMS：初代并发、低延迟
第一款**并发**收集器（不是并行哦！）
2022年9月15日
19:03

CMS第一次实现了让垃圾收集线程与用户线程同时工作
核心理念：尽可能缩短垃圾收集时的停顿时间，低延迟

不幸的是，CMS作为老年代的收集器，却无法与JDK1.4.0中已经存在的新生代收集器Parallel Scavenge配合工作（因为实现的框架不一样，没办法兼容使用），所以在JDK1.5中使用CMS来收集老年代的时候，新生代只能选择ParNew或者Serial收集器中的一个。

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)
并发性：可以和用户线程同时执行
