---
title: wait、notify、interrupt
updated: 2021-09-24T16:28:49
created: 2021-09-24T11:36:56
---

wait：使当前调用该方法的线程释放进入等待状态。
wait(long time)：等待，超时后自动唤醒。
调用wait方法前：必须先获得锁对象。

调用wait方法时：当前线程会立即释放锁。

调用wait方法后：只要不被notify，就会永远等待。

notify/notifyall：通知等待的任一/所有线程停止睡眠。
notify必须在同步代码块中，由锁对象调用！

notify调用后，不会立即释放锁，必须等当前同步代码块执行完毕后才能释放锁（所以一般放在同步代码块最后，sleep之前）

interrupt：实例方法，打断某线程对象。在一个线程中调用另一个线程的interrupt()方法，即会向那个线程发出信号：线程中断状态已被设置。
如果被打断的线程处于wait、sleep状态下，则会停止或被打断。

不能中断在运行中的线程，它只能改变中断状态而已。

java.lang.IllegalMonitorStateException：线程未持有对象A的锁，却调用了对象A的wait()方法。（因为线程此时执行wait无锁可释放！）
public static void main(String\[\] args) {
Object obj1 = new Object();
Object obj2 = new Object();
synchronized (obj1) {
try {
obj2.wait(); //线程持有obj1的锁，但是却尝试释放obj2的锁，而线程根本就没有obj2的锁，因此抛出异常。
} catch (InterruptedException e) {
e.printStackTrace();
}
}
}

