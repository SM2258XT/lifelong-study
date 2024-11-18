---
title: ExecutorService常用方法
updated: 2023-03-03T12:11:50
created: 2022-10-25T13:55:59
---

[挖坟回忆](onenote:#ThreadPoolExecutor：线程池&section-id={CDD07D22-AD2B-45C8-A76D-8BE3375113E8}&page-id={DCC051FA-0BE9-4437-81D5-C5374AE2981A}&end&base-path=https://d.docs.live.net/36a2ce0fd7a6557d/文档/Java/Java卷王之路.one)
1.  **关闭**
shutdown：顺序关闭，继续执行剩余任务，但不接受新任务。

shutdownNow：调用活动线程的 interrupt() 进行中断，并返回剩余任务集合 List\<Runnable\>。
1.  **任务提交和执行**
/\*\*

\* 提交一个返回值的任务用于执行，返回一个表示任务的未决结果的 Future

\*/

\<T\> Future\<T\> submit(Callable\<T\> task);

/\*\*

\* 提交一个 Runnable 任务用于执行，并返回一个表示该任务的 Future

\*/

\<T\> Future\<T\> submit(Runnable task, T result);

/\*\*

\* 提交一个 Runnable 任务用于执行，并返回一个表示该任务的 Future

\*/

Future\<?\> submit(Runnable task);

/\*\*

\* 执行给定的任务，当所有任务完成时，返回保持任务状态和结果的 Future 列表

\*/

\<T\> List\<Future\<T\>\> invokeAll(Collection\<? extends Callable\<T\>\> tasks) throws InterruptedException;

/\*\*

\* 执行给定的任务，当所有任务完成或超时期满时（无论哪个首先发生），返回保持任务状态和结果的 Future 列表

\*/

\<T\> List\<Future\<T\>\> invokeAll(Collection\<? extends Callable\<T\>\> tasks, long timeout, TimeUnit unit) throws InterruptedException;

/\*\*

\* 执行给定的任务，如果某个任务已成功完成（也就是未抛出异常），则返回其结果

\*/

\<T\> T invokeAny(Collection\<? extends Callable\<T\>\> tasks) throws InterruptedException, ExecutionException;

/\*\*

\* 执行给定的任务，如果在给定的超时期满前某个任务已成功完成（也就是未抛出异常），则返回其结果

\*/

\<T\> T invokeAny(Collection\<? extends Callable\<T\>\> tasks, long timeout, TimeUnit unit) throws InterruptedException, ExecutionException, TimeoutException;
