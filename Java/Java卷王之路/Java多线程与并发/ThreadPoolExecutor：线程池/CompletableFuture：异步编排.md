---
title: '*** CompletableFuture：异步编排'
updated: 2022-11-20T14:19:05
created: 2022-09-10T18:13:00
---

\*\*\* CompletableFuture：异步编排
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
2022年9月10日
18:13

**强大之处：**
1.  提供异步回调CALLBACK。
包括任务完成CALLBACK、异常捕捉CALLBACK
1.  可以是组合的多个CompletionStage
CompletionStage代表计算中的某个阶段。

**使用：**[使用CompletableFuture](onenote:项目实战2.one#线程任务组合&section-id={1F966D02-1500-4670-960F-DC885958D490}&page-id={DAA45C21-9976-49BE-A995-0E6021C02D3E}&end&base-path=https://d.docs.live.net/36a2ce0fd7a6557d/文档/Java)

分类：
1.  获取结果
get()、join()、getNow(valueIfAbsent)、complete(value)等
1.  get和join的唯一区别在于：get必须try-catch，而join不用
2.  complete(value)打断计算，直接返回指定值
1.  结果处理
    - thenApply()：串行继续，有异常全部停止
    - handle()：串行继续，有异常处理
2.  结果消费
    - thenAccept(Consumer c)：在Comsumer函数里就消费了，无返回值。
    - thenApply()：有返回值，返回后供其他线程消费。
3.  结果合并
。。。
1.  速度选用
。。。

**特别注意：**
1.  **默认ForkJoinPool是守护线程池。**
    - **如果创建时未指定自定义线程池，则会使用内置ForkJoinPool。而这是一个守护线程池，也就是说如果main在计算未完成之前就结束，则ForkJoinPool不管状态如何都会结束！！**
    - **99%的情况下，都应该手动指定线程池，并且注意自定义线程池用完了记得关闭。**
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)
