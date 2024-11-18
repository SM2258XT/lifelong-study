---
title: StampedLock：邮戳锁
updated: 2022-09-11T19:07:17
created: 2022-09-11T18:53:02
---

**相关锁的历史演变历程**
无锁 -\> 同步锁（synchronized） -\> 读写锁（ReadWriteLock） -\> 邮戳锁（StampedLock，JDK1.8）
主要解决的问题：原ReadWriteLock为悲观锁，而且为了解决线程饥饿的问题设置为公平锁后，仍然会消耗许多性能。

**StampedLock为乐观锁**
当读锁被占用时，其他线程获取写锁不会阻塞（别人正在读时，自己可以写）。
因此，在获取乐观读锁后，需要对结果进行校验。
有一个stamp变量（戳记，long类型）。当使用tryOptimisticRead() 方法（乐观读），读取完毕后需要做一次 戳校验 ，如果校验通过，表示这期间确实没有写操作，数据可以安全使用，如果校验没通过，需要重新获取读锁，保证数据安全。

**特点**
1.  乐观锁
2.  不可重入锁

所有获取锁的方法，都返回一个邮戳（Stamp），Stamp为零表示获取失败，其余都表示成功；
所有释放锁的方法，都需要一个邮戳（Stamp），这个Stamp必须是和成功获取锁时得到的Stamp一致；
StampedLock是不可重入的，危险(如果一个线程已经持有了写锁，再去获取写锁的话就会造成死锁)
StampedLock有三种访问模式
①Reading（读模式）：功能和ReentrantReadWriteLock的读锁类似
②Writing（写模式）：功能和ReentrantReadWriteLock的写锁类似
③Optimistic reading（乐观读模式）：无锁机制，类似于数据库中的乐观锁，
支持读写并发，很乐观认为读取时没人修改，假如被修改再实现升级为悲观读模式
