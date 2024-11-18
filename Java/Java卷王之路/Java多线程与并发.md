---
title: Java多线程与并发
updated: 2024-09-03T16:26:31
created: 2022-05-12T17:49:44
---

1.  Java实现线程有哪几种方式？
本质上只有两种：Runnable.run()、Callable.call()

衍生出来的：继承Thread、FutureTask、Executor、CompletableFuture

1.  启动线程方法**start()**和 run()有什么区别？
    - 只有调用了start()方法，才会表现出多线程的特性，不同线程的run()方法里面的代码交替执行。如果只是调用run()方法，那么代码还是同步执行的，必须等待一个线程
的 run()方法里面的代码全部执行完毕之后，另外一个线程才可以执行其 run()方法里面的代码。
1.  只有调用start()才是真正的开辟了新线程
1.  线程状态
public enum State { // Thread.State

NEW, // 还没调用Run()

RUNNABLE, // 已经触发start()方式调用，线程正式启动，处于运行中状态。

BLOCKED, // 阻塞，等待获取锁

WAITING, // 处于无限制等待状态，等待一个特殊的事件来重新唤醒（例如join()、wait的线程需要notify、notifyAll）

TIMED_WAITING, // 进入了一个有时限的等待，如 sleep(3000)

TERMINATED; // 执行完毕后，进行终止状态。终止后也不能再回到RUNNABLE 状态

}
1.  **wait()和sleep()方法有什么区别？**
    - **相同点：都会暂停当前线程并让出cpu的执行时间**
    - **不同点：sleep不会释放当前持有的对象的锁资源，到时间后会继续执行，而wait会放弃所有锁并需要notify/notifyAll后重新获取到对象锁资源后才能继续执行。**
sleep可以在任何地方使用，而wait只能在同步方法或者同步块中使用。

sleep是Thread的静态方法，wait是Object的方法，任何对象实例都能调用。
1.  新建 T1、T2、T3 三个线程，如何保证它们按顺序执行？
用 join 方法。

