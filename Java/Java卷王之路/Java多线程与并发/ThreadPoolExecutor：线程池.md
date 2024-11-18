---
title: ThreadPoolExecutor：线程池
updated: 2022-11-21T07:51:52
created: 2021-09-25T11:04:51
---

**构造函数的参数解释：**
1.  corePoolSize：表示允许线程池中允许**同时运行**的最大线程数。
线程池创建之后，线程池中的线程数为0，当任务过来就会创建一个线程去执行，直到线程数达到corePoolSize 之后，就会把更多到达的任务放在队列中。
1.  maximumPoolSize：线程池允许的最大线程数，他表示最大能创建多少个线程。maximumPoolSize肯定是大于等于corePoolSize。
2.  keepAliveTime：表示线程没有任务时最多保持多久然后停止。默认情况下，只有线程池中线程数大于corePoolSize时，keepAliveTime 才会起作用。换句话说，当线程池中的线程数大于corePoolSize，并且一个线程空闲时间达到了keepAliveTime，那么就会结束该线程。
3.  Unit：keepAliveTime的单位。
4.  workQueue：一个阻塞队列，用来存储等待执行的任务，当线程池中的线程数超过它的corePoolSize的时候，线程会进入阻塞队列进行阻塞等待。通过workQueue，线程池实现了阻塞功能。
5.  threadFactory ：线程工厂，用来创建线程。
6.  handler :表示当拒绝处理任务时的策略。

**线程池的任务处理策略：**
1.  如果当前线程池中的线程数目小于corePoolSize，则每来一个任务，就会创建一个线程去执行这个任务；
2.  如果当前线程池中的线程数目\>=corePoolSize，则每来一个任务，会尝试将其添加到任务缓存队列当中，若添加成功，则该任务会等待空闲线程将其取出去执行；若添加失败（一般来说是任务缓存队列已满），则会尝试创建新的线程去执行这个任务；如果当前线程池中的线程数目达到maximumPoolSize，则会采取任务**拒绝策略**进行处理；
3.  如果线程池中的线程数量大于 corePoolSize时，如果某线程空闲时间超过keepAliveTime，线程将被终止，直至线程池中的线程数目不大于corePoolSize；如果允许为核心池中的线程设置存活时间，那么核心池中的线程空闲时间超过keepAliveTime，线程也会被终止。

**拒绝策略：**
1.  DiscardPolicy：丢弃不处理。
2.  AbortPolicy：丢弃抛异常，RejectedExecutionException
3.  CallerRunsPolicy：不丢弃，直接在请求者的线程中执行。
只要线程池未关闭，该策略直接在请求者线程中，运行当前被丢弃的任务。显然这样做不会真的丢弃任务，但是，任务提交线程的性能极有可能会急剧下降。
1.  DiscardOldestPolicy：不丢弃，但丢弃队列中最老的一个请求，也就是即将被执行的一个任务（因为FIFO哦！），并尝试再次提交当前任务（而不是直接执行）。

**线程池的关闭**
ThreadPoolExecutor提供了两个方法，用于线程**池**的关闭（而不是线程的终止），分别是shutdown()和shutdownNow()，其中：
1.  shutdown()：不会立即终止线程池，而是要等所有任务缓存队列中的任务都执行完后才终止，但再也不会接受新的任务。
2.  shutdownNow()：立即终止线程池，并尝试打断正在执行的任务，并且清空任务缓存队列，返回尚未执行的任务。
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image3.png)
