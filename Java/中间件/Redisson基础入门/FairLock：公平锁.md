---
title: FairLock：公平锁
updated: 2022-10-27T19:07:05
created: 2022-10-27T19:04:35
---

保证多个线程同时请求锁时，按照请求顺序分配锁，最多等待5秒。例如有3个线程同时请求时，最后一个线程最长等待15秒。

RLock fairLock = redisson.getFairLock("anyLock");
// 最常见的使用方法
fairLock.lock();

// 10秒钟以后自动解锁
// 无需调用unlock方法手动解锁
fairLock.lock(10, TimeUnit.SECONDS);

// 尝试加锁，最多等待100秒，上锁以后10秒自动解锁
boolean res = fairLock.tryLock(100, 10, TimeUnit.SECONDS);
fairLock.unlock();
