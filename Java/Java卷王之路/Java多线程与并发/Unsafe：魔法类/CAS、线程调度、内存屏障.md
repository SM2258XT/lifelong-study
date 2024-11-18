---
title: CAS、线程调度、内存屏障
updated: 2022-11-21T19:46:12
created: 2022-11-21T19:02:09
---

CAS、线程调度、内存屏障
<https://tech.meituan.com/2019/02/14/talk-about-java-magic-class-unsafe.html>
2022年11月21日
19:02

**CAS支持**
CAS由硬件保证，由Unsafe桥接
CAS是一条CPU原语实现的（cmpxchg），由硬件保证，一定不会造成所谓的数据不一致问题。
存在意义：java不能直接调用原语，而是通过Unsafe类中native方法，让底层C/C++实现。这个Unsafe类更像是一个桥接类，负责连接java和c/c++，大多数都是native方法。
Atomic原子类，大多数都是在调用Unsafe类方法。

CAS在java.util.concurrent.atomic相关类、Java AQS、CurrentHashMap等实现上有非常广泛的应用。如下图所示，AtomicInteger的实现中，静态字段valueOffset即为字段value的内存偏移地址，valueOffset的值在AtomicInteger初始化时，在静态代码块中通过Unsafe的objectFieldOffset方法获取。在AtomicInteger中提供的线程安全方法中，通过字段valueOffset的值可以定位到AtomicInteger对象中value的内存地址，从而可以根据CAS实现对value字段的原子操作。

**线程调度**
方法park、unpark即可实现线程的挂起与恢复，将一个线程进行挂起是通过park方法实现的，调用park方法后，线程将一直阻塞直到超时或者中断等条件出现；unpark可以终止一个挂起的线程，使其恢复正常。
Java锁和同步器框架的核心类AbstractQueuedSynchronizer，就是通过调用LockSupport.park()和LockSupport.unpark()实现线程的阻塞和唤醒的，而LockSupport的park、unpark方法实际是调用Unsafe的park、unpark方式来实现。

**内存屏障**
定义内存屏障，CPU或编译器在对内存随机访问的操作中的一个同步点，使得此点之前的所有读写操作都执行后才可以开始执行此点之后的操作，避免代码重排序。
应用：StampedLock邮戳锁
StampedLock提供了一种乐观读锁的实现，这种乐观读锁类似于无锁的操作，完全不会阻塞写线程获取写锁，从而缓解读多写少时写线程“饥饿”现象
校验锁状态这步操作至关重要，需要判断锁状态是否发生改变，从而判断之前copy到线程工作内存中的值是否与主内存的值存在不一致。
在校验逻辑之前，会通过Unsafe的loadFence方法加入一个load内存屏障，目的是避免上图用例中步骤②和StampedLock.validate中锁状态校验运算发生重排序导致锁状态校验不准确的问题。
