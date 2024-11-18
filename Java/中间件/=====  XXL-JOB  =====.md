---
title: =====  XXL-JOB  =====
updated: 2022-10-13T16:05:58
created: 2022-10-12T15:48:08
---

===== XXL-JOB =====
<https://www.iocoder.cn/XXL-JOB/install/?self>
2022年10月12日
15:48

**定时任务选型：**
1.  在 JDK 中，内置了两个类，可以实现定时任务的功能，但基本不用了：java.util.Timer、java.util.concurrent.ScheduledExecutorService
2.  Spring Framework 的 **Spring Task** 模块，提供了轻量级的定时任务的实现。
3.  Spring Boot 2.0 版本，整合了 **Quartz** 作业调度框架，提供了功能强大的定时任务的实现。
4.  Elastic-Job
1.  **XXL-JOB**
目前国内采用 Elastic-Job 和 XXL-JOB 为主。使用 XXL-JOB 的团队会更多一些，主要是上手较为容易，运维功能更为完善。
