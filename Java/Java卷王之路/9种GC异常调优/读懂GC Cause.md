---
title: 读懂GC Cause
updated: 2022-11-20T16:17:51
created: 2022-11-20T14:56:58
---

拿到 GC 日志，我们就可以简单分析 GC 情况了

**以下常见几种GC Cause**
System.gc()： 手动触发GC操作。
CMS： CMS GC 在执行过程中的一些动作，重点关注 CMS Initial Mark 和 CMS Final Remark 两个 STW 阶段。
Promotion Failure： Old 区没有足够的空间分配给 Young 区晋升的对象（即使总可用内存足够大）。
Concurrent Mode Failure： CMS GC 运行期间，Old 区预留的空间不足以分配给新的对象，此时收集器会发生退化，严重影响 GC 性能，下面的一个案例即为这种场景。
GCLocker Initiated GC： 如果线程执行在 JNI 临界区时，刚好需要进行 GC，此时 GC Locker 将会阻止 GC 的发生，同时阻止其他线程进入 JNI 临界区，直到最后一个线程退出临界区时触发一次 GC。

另外，通过一些工具，我们可以比较直观地看到 Cause 的分布情况，比如使用 gceasy 绘制图表

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
