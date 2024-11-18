---
title: RedLock：红锁
updated: 2022-10-27T19:11:37
created: 2022-10-27T19:10:52
---

RedissonRedLock对象实现了红锁的加锁算法。该对象也可以用来将多个\`RLock\`对象关联为一个红锁，每个\`RLock\`对象实例可以来自于不同的Redisson实例。

RLock lock1 = redissonInstance1.getLock("lock1");
RLock lock2 = redissonInstance2.getLock("lock2");
RLock lock3 = redissonInstance3.getLock("lock3");

RedissonRedLock lock = new RedissonRedLock(lock1, lock2, lock3);
// 同时加锁：lock1 lock2 lock3
// 红锁在大部分节点上加锁成功就算成功。
lock.lock();
...
lock.unlock();

