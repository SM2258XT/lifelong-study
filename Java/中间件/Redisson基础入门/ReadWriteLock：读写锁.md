---
title: ReadWriteLock：读写锁
updated: 2022-10-27T19:12:33
created: 2022-10-27T19:11:41
---

分布式可重入读写锁RReadWriteLock对象，实现了java.util.concurrent.locks.ReadWriteLock接口。
其中读锁和写锁都继承了RLock。
分布式可重入读写锁允许同时有多个读锁和一个写锁处于加锁状态。

RReadWriteLock rwlock = redisson.getReadWriteLock("anyRWLock");
// 最常见的使用方法
rwlock.readLock().lock();
// 或
rwlock.writeLock().lock();

// 10秒钟以后自动解锁
// 无需调用unlock方法手动解锁
rwlock.readLock().lock(10, TimeUnit.SECONDS);
// 或
rwlock.writeLock().lock(10, TimeUnit.SECONDS);

// 尝试加锁，最多等待100秒，上锁以后10秒自动解锁
boolean res = rwlock.readLock().tryLock(100, 10, TimeUnit.SECONDS);
// 或
boolean res = rwlock.writeLock().tryLock(100, 10, TimeUnit.SECONDS);
...
lock.unlock();

