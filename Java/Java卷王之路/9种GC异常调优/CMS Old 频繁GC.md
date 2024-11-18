---
title: CMS Old 频繁GC
updated: 2022-11-21T07:00:43
created: 2022-11-20T20:21:17
---

**现象：**单次耗时并不算长，整体STW时间也可接受，但是由于GC太频繁导致吞吐量下降很多。
**原因：**
比较常见，基本是因为YGC完成后，负责处理CMS GC（Old区）的后台线程concurrentMarkSweepThread会不断地轮询，使用shouldConcurrentCollect()方法做一次检测，判断是否达到了回收条件。间隔周期为 -XX:CMSWaitDuration 决定，默认为2s。

其中这个判断逻辑：
1.  可以触发CMS GC：
2.  可以触发Full GC

未完待续……
