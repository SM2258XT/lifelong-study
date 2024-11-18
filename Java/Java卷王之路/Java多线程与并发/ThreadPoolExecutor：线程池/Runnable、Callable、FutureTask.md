---
title: Runnable、Callable、FutureTask
updated: 2022-11-12T08:32:32
created: 2022-05-15T17:53:13
---

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
Runnable、Callable、FutureTask
2022年5月15日
17:53

|          |          |          |
|----------|----------|----------|
|         | Runnable | Callable |
| 返回值？ | /        | 有       |
| 抛异常？ | /        | 有       |

**Callable不能直接通过Thread创建线程**
new Thread（runnable）：可以
new Thread（callable）：不行。因为根本没有这个构造方法。
本质上是因为Thread类里保存线程任务的属性是：private Runnable target;，并且thread.start()调用的是Runnable.**run**()，但是Callable唯一的方法是**call()**
**使用FutureTask桥接模式创建线程**
new Thread（new FutureTask（new Callable（）））
值得一提的是，Thread这里实际调用的仍然是Thread(**Runnable target**)构造函数，因为FutureTask其实是Runnable接口的实现类。

**常用操作**
1.  **futuretask.get()：**阻塞获取未来计算结果
调用get方法时，会阻塞调用者线程，直至task线程计算完毕返回结果。
1.  **futuretask.isDone()：**判断是否完成

**使用技巧**
1.  与线程池组合使用：excutorService.submit(futuretask);
2.  与异步编排组合使用：
**缺点：**
1.  使用get()导致阻塞
2.  使用isDone()轮询耗费CPU
**总结：**虽然相较于Runnable有了异常获取、返回结果的提升，但是产生了以上新问题。
**解决：**升级使用CompletableFuture
