---
title: 运行时数据区Runtime
updated: 2022-09-13T12:10:21
created: 2022-05-04T17:38:47
---

每个JVM只有一个单例Runtime，用来与JVM中的运行时数据区进行交互

JVM 系统线程
主要的后台系统线程在Hotspot JVM里主要是以下几个：
1.  虚拟机线程：这种线程的操作是需要JVM达到安全点才会出现。这些操作必须在不同的线程中发生的原因是他们都需要JVM达到安全点，这样堆才不会变化。这种线程的执行类型括"stop-the-world"的垃圾收集，线程栈收集，线程挂起以及偏向锁撤销
2.  周期任务线程：这种线程是时间周期事件的体现（比如中断），他们一般用于周期性操作的调度执行
3.  GC线程：这种线程对在JVM里不同种类的垃圾收集行为提供了支持
4.  编译线程：这种线程在运行时会将字节码编译成到本地代码
5.  信号调度线程：这种线程接收信号并发送给JVM，在它内部通过调用适当的方法进行处理

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)
