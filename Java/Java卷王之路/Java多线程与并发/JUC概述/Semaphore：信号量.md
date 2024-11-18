---
title: Semaphore：信号量
updated: 2022-09-10T12:53:57
created: 2022-09-10T10:26:24
---

**Semaphore概念：**信号量机制、PV操作

**例：**30辆车抢占3个车位
Semaphore semaphore = **new Semaphore(3)**;
for (int i = 0; i \< 30; i++) {
new Thread(() -\> {
try {
semaphore.**acquire()**;
System.out.println("第" + Thread.currentThread().getName() + "辆车抢到了车位");
TimeUnit.SECONDS.sleep(new Random().nextInt(3));
} catch (InterruptedException e) {
throw new RuntimeException(e);
} **finally** {
semaphore**.release()**; // 尽量在finally中进行release，避免程序出错而没有释放锁（finally中，无论怎样都会执行）
System.out.println("第" + Thread.currentThread().getName() + "辆车离开");
}
}, i + "").start();
}

