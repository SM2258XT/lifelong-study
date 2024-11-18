---
title: ParNew
updated: 2022-09-16T08:18:27
created: 2022-09-15T15:52:32
---

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
ParNew（并行，新生代 垃圾回收器）

ParNew 和 Serial 的**唯一区别**就是采用了**并行回收**方式
也是复制算法，STW

**ParNew 不一定比 Serial 性能好**
在单个CPU的环境下，ParNew收集器不比Serial收集器更高效。虽然Serial收集器是基于串行回收，但是单核CPU不需要频繁地做任务切换，因此可以有效避免多线程交互过程中产生的额外开销。

除Serial外，目前只有ParNew GC能与CMS收集器配合工作

