---
title: AQS：队列同步器
updated: 2022-11-21T10:20:24
created: 2022-09-10T07:35:51
---

AQS：队列同步器
2022年9月10日
7:35

**锁和同步器的概念**
锁：面向程序员。提供锁相关API，对程序员隐藏了内部细节。
同步器：面向不同锁的实现者。提出统一规范并简化锁的实现，屏蔽了实现锁需要关注的同步状态管理、队列维护、线程阻塞排队通知、唤醒机制等一些列底底底的公共基础。
java.util.concurrent.locks.AbstractQueuedSynchronizer
AQS是构建锁或其他同步组件的基础框架，使用方式：子类继承同步器，并实现它的抽象方法来管理同步状态。

从ReentrantLock的实现看AQS的原理及应用 <https://tech.meituan.com/2019/12/05/aqs-theory-and-apply.html>
