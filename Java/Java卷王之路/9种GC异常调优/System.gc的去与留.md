---
title: System.gc的去与留
updated: 2022-11-20T19:52:48
created: 2022-11-20T16:30:28
---

System.gc会引发一次Full GC，进行整堆收集
-XX:+DisableExplicitGC可以将其关闭，原理是当System.gc执行时，对应的C语言清理方法，会使用空方法来替换原有的Universe::heap()::collect

**如果保留**
可以，但不要频繁使用。
CMS GC分为Background和Foreground两种模式，前者是常规理解的并发收集，不怎么影响业务线程。但Foreground Collector却会进行一次**压缩式GC**，使用的是和Serial Old一样的标记压缩Lisp2算法，这个相比其他算法代价更大，会引发更长的STW。因此System.gc不要频繁使用！

**如果去除**
会带来**内存泄漏**问题。
DirectByteBuffer有着零拷贝等特点，使用堆外内存（Native Memory），不被JVM管理必须手动释放，并且没有Finalizer，而也是通过sun.msic.Cleaner完成的
重点：为 DirectByteBuffer 分配空间过程中会显式调用 System.gc ，希望通过 Full GC 来强迫已经无用的 DirectByteBuffer 对象释放掉它们关联的 Native Memory
HotSpot VM 只会在 Old GC 的时候才会对 Old 中的对象做 Reference Processing，而在 Young GC 时只会对 Young 里的对象做 Reference Processing。
Young 中的 DirectByteBuffer 对象会在 Young GC 时被处理，也就是说，做 CMS GC 的话会对 Old 做 Reference Processing，进而能触发 Cleaner 对已死的 DirectByteBuffer 对象做清理工作。但如果很长一段时间里没做过 GC 或者只做了 Young GC 的话则不会在 Old 触发 Cleaner 的工作，那么就可能让本来已经死亡，但已经晋升到 Old 的 DirectByteBuffer 关联的 Native Memory 得不到及时释放

**总结：建议保留**
因为当前大量RPC通信都会使用NIO
不过可以使用参数-XX:+ExplicitGCInvokesConcurrent 和 -XX:+ExplicitGCInvokesConcurrentAndUnloadsClasses 参数来将 System.gc 的触发类型从 Foreground 改为 Background，同时 Background 也会做 Reference Processing，这样的话就能大幅降低了 STW 开销，同时也不会发生 NIO Direct Memory OOM。

