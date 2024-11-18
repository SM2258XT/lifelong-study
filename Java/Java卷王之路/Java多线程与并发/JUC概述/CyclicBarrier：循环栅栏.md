---
title: CyclicBarrier：循环栅栏
updated: 2022-09-10T21:04:11
created: 2022-09-10T08:59:28
---

CyclicBarrier：循环栅栏
2022年9月10日
8:59

**CyclicBarrier概念**
若干个线程，只有它们全部到达了某一个点之后，才能同时继续一起执行。  
所有子线程都已经到达屏障之后，此时屏障就会消失，所有子线程继续执行，若存子线程尚未到达屏障，其他到达了屏障的线程都会进行等待。
**应用场景**
适用于**多线程结果合并**的操作，用于多线程计算数据，最后合并计算结果的应用场景。比如，我们需要统计多个 Excel 中的数据，然后等到一个总结果。我们可以通过多线程处理每一个 Excel ，执行完成后得到相应的结果，最后通过 barrierAction 来计算这些线程的计算结果，得到所有Excel的总和。

**主要关注几个内部参数：**
1.  **parties**，表示拦截线程的总数量。一般在构造方法传入。
2.  **count**，表示拦截线程的剩余需要数量。
3.  **barrierAction**，为 CyclicBarrier 接收的 Runnable 命令，用于在线程到达屏障时，优先执行 barrierAction ，用于处理更加复杂的业务场景。
4.  **generation**，表示 CyclicBarrier 的更新换代。

如果该线程**不是**到达的最后一个线程，则他会一直处于阻塞状态，除非发生以下情况：
- 最后一个线程到达，即 index == 0 。
- 超出了指定时间（超时等待）。
- 其他的某个线程中断当前线程，或者其他等待的线程。
其他所有线程都将抛出 BrokenBarrierException 异常，并将 barrier 置于损坏状态。
- 其他的某个线程在等待 barrier 超时。
- 其他的某个线程在此 barrier 调用 \#reset() 方法。#reset() 方法，用于将屏障重置为初始状态。

**Generation**
- Generation 是 CyclicBarrier 内部静态类，描述了 CyclicBarrier 的更新换代。在CyclicBarrier中，同一批线程属于同一代。当有 parties 个线程全部到达 barrier 时，generation 就会被更新换代。其中 broken 属性，标识该当前 CyclicBarrier 是否已经处于中断状态。当 barrier 损坏了，或者有一个线程中断了，则通过 \#breakBarrier() 方法，来终止所有的线程，将在 CyclicBarrier 处于等待状态的线程全部唤醒。
- 当所有线程都已经到达 barrier 处（index == 0），则会通过 nextGeneration() 方法，进行更新换代操作。在这个步骤中，做了三件事：
唤醒所有线程。

重置 count 。

重置 generation 。

**例：集齐7颗龙珠，才可以召唤神龙！**
CyclicBarrier cyclicBarrier = new CyclicBarrier(**NUM**, () -\> {
System.out.println("集齐7颗龙珠，才可以召唤神龙！");
});
for (int i = 0; i \< NUM; i++) {
new Thread(() -\> {
try {
System.out.println(Thread.currentThread().getName() + "星龙珠被收集到了！");
cyclicBarrier**.await()**;
System.out.println(Thread.currentThread().getName() + "星龙珠被使用");
} catch (Exception e) {
throw new RuntimeException(e);
}
}, String.valueOf(i)).start();
}
