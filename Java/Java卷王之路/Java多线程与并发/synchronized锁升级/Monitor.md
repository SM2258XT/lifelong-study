---
title: Monitor
updated: 2022-09-16T11:04:10
created: 2022-09-06T09:49:26
---

Monitor
2022年9月6日
9:49

**synchronized字节码层面的实现原理**
同步代码块：monitorenter和monitorexit指令
任何对象都有一个 Monitor 与之相关联。当且一个 Monitor 被持有之后，他将处于锁定状态。

线程执行到 monitorenter 指令时，将会尝试获取对象所对应的 Monitor 所有权
同步方法：方法修饰符上的ACC_SYNCHRONIZED
Class 文件的方法表中，将该方法的 access_flags 字段中的 synchronized 标志位置设置为 1

**Monitor Record 是线程私有的数据结构**
每一个线程都有一个可用 Monitor Record 列表，同时还有一个全局的可用列表。
每一个被锁住的对象都会和一个 Monitor Record 关联（对象头的 MarkWord 中的 LockWord 指向 Monitor 的起始地址），Monitor Record 中有一个 Owner 字段，存放拥有该锁的线程的唯一标识，表示该锁被这个线程占用。）
Owner：1）初始时为 NULL 表示当前没有任何线程拥有该 Monitor Record；2）当线程成功拥有该锁后保存线程唯一标识；3）当锁被释放时又设置为 NULL 。
EntryQ：关联一个系统互斥锁（ semaphore ），阻塞所有试图锁住 Monitor Record失败的线程 。
RcThis：表示 blocked 或 waiting 在该 Monitor Record 上的所有线程的个数。
Nest：用来实现重入锁的计数。
HashCode：保存从对象头拷贝过来的 HashCode 值（可能还包含 GC age ）。
Candidate：用来避免不必要的阻塞或等待线程唤醒。因为每一次只有一个线程能够成功拥有锁，如果每次前一个释放锁的线程唤醒所有正在阻塞或等待的线程，会引起不必要的上下文切换（从阻塞到就绪然后因为竞争锁失败又被阻塞）从而导致性能严重下降。Candidate 只有两种可能的值 ：1）0 表示没有需要唤醒的线程；2）1 表示要唤醒一个继任线程来竞争锁。

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
