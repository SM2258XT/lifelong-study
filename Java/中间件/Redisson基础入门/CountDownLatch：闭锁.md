---
title: CountDownLatch：闭锁
updated: 2022-10-27T19:13:47
created: 2022-10-27T19:13:01
---

RCountDownLatch采用了与java.util.concurrent.CountDownLatch相似的接口和用法。

RCountDownLatch latch = redisson.getCountDownLatch("anyCountDownLatch");
latch.trySetCount(1);
latch.await();

// 在其他线程或其他JVM里
RCountDownLatch latch = redisson.getCountDownLatch("anyCountDownLatch");
latch.countDown();

