---
title: 堆外OOM
updated: 2022-11-21T10:17:37
created: 2022-11-21T09:45:52
---

堆外OOM
2022年11月21日
9:45

内存使用率不断上升，甚至开始使用 SWAP 内存，同时可能出现 GC 时间飙升，线程被 Block 等现象，通过 top 命令发现 Java 进程的 RES 甚至超过了 -Xmx 的大小。出现这些现象时，基本可以确定是出现了堆外内存泄漏。

**主要原因**
1.  堆外内存申请没释放
通过 UnSafe#allocateMemory，ByteBuffer#allocateDirect 主动申请了堆外内存而没有释放，常见于 NIO、Netty 等相关组件。

使用 -XX:MaxDirectMemorySize=size 参数来控制可申请的堆外内存的最大值。在 Java8 中，如果未配置该参数，默认和 -Xmx 相等。

NIO 和 Netty 都会取 -XX:MaxDirectMemorySize 配置的值，来限制申请的堆外内存的大小，还有一个计数器字段，用来计算当前已申请的堆外内存大小，NIO 中是 java.nio.Bits#totalCapacity、Netty 中 io.netty.util.internal.PlatformDependent#DIRECT_MEMORY_COUNTER。

当申请堆外内存时，NIO 和 Netty 会比较计数器字段和最大值的大小，如果计数器的值超过了最大值的限制，会抛出 OOM 的异常。

我们可以检查代码中是如何使用堆外内存的，NIO 或者是 Netty，通过反射，获取到对应组件中的计数器字段，并在项目中对该字段的数值进行打点，即可准确地监控到这部分堆外内存的使用情况。

此时，可以通过 Debug 的方式确定使用堆外内存的地方是否正确执行了释放内存的代码。另外，需要检查 JVM 的参数是否有 -XX:+DisableExplicitGC 选项，如果有就去掉，因为该参数不去掉会使 System.gc 失效。
1.  代码中通过JNI调用的Native Code申请的内存没释放
这种情况排查起来比较困难，我们可以通过 Google perftools + Btrace 等工具，帮助我们分析出问题的代码在哪里。

Netty堆外内存溢出案例：https://tech.meituan.com/2018/10/18/netty-direct-memory-screening.html
