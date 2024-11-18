---
title: MultiLock：关联锁
updated: 2022-10-27T19:10:49
created: 2022-10-27T19:09:04
---

可以将多个RLock关联为一个关联锁，每个RLock可能来自不同Redisson客户端。

RLock lock1 = redissonInstance1.getLock("lock1");
RLock lock2 = redissonInstance2.getLock("lock2");
RLock lock3 = redissonInstance3.getLock("lock3");

RedissonMultiLock lock = new RedissonMultiLock(lock1, lock2, lock3); // 同时加锁：lock1 lock2 lock3，所有的锁都上锁成功才算成功。
lock.lock();
...
lock.unlock();
