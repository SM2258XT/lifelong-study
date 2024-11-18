---
title: CountDownLatch：减小计数
updated: 2022-09-10T09:22:12
created: 2022-09-10T08:54:25
---

CountDownLatch：减小计数
2022年9月10日
8:54

**CountDownLatch：**指定初始计数，使用await()方法阻塞当前线程，直至计数为0
**例：**模拟所有同学都全部离开后，班长才能锁门走人。

final int NUMS = 60;
CountDownLatch countDownLatch = **new CountDownLatch(<u>NUMS</u>)**;
for (int i = 0; i \< NUMS; i++) {
new Thread(() -\> {
System.out.println(Thread.currentThread().getName() + "同学离开教室");
**countDownLatch.<u>countDown</u>(); // 减小计数**
}, i + "").start();
}
**countDownLatch.<u>await</u>(); // 阻塞main线程，直至所有同学离开**
System.out.println("班长锁门走人了");

如果没有使用CountDownLatch（或者其他线程同步机制），则很可能还有同学没离开，门就已经上锁了，错误。

**特别注意：**
这个例子并没有演示完整，其实只额外开一个线程，循环执行N次countDown()之后，main仍然可以结束await！
**CountDownLatch真的只是单纯countDown**！！！和其他线程并无关系

这样也可以：
for (int i = 0; i \< NUMS; i++) {
**new Thread**(() -\> { // 只开了一个线程，用于减少计数
System.out.println(Thread.currentThread().getName() + "离开教室");
countDownLatch.countDown();
}, i + "").start();
}
countDownLatch.await();
