---
title: MVCC概述、当前读和快照读
updated: 2022-11-13T14:59:46
created: 2022-10-17T14:49:58
---

**两种SELECT：**

SELECT ... LOCK IN SHARE MODE　 （共享锁）

SELECT ... FOR UPDATE （悲观锁）

触发条件：

只有最普通的SELECT是LOCK IN SHARE MODE

除此之外，以下全为FOR UPDATE：

insert、update、delete、select x for update、select x lock in share mode？？？

**当前读和快照读**

当引入MVCC后，多了一种快照读的概念
1.  当前读：需要满足SELECT ... FOR UPDATE触发
读取的一定是记录最新版本，并同时保证不被其他事务修改，连读取都会加锁。
1.  快照读：需要满足SELECT ... LOCK IN SHARE MODE
读取的不一定是最新版本，但不会加锁，因此并发性能好，是MVCC的核心思想。

最最普通的select是快照读

总结：先是看SELECT是哪种类型，然后再因为MVCC，所以把其中乐观锁的SELECT全部变成了快照读。

**MVCC解决的问题**

传统数据库并发场景3种：
1.  RR并发
无安全问题
1.  **RW并发**
脏读、幻读、不可重复读
1.  WW并发
**更新丢失**

MVCC提升的仅是RW安全并发性能，将传统的有锁当前读，升级为**无锁快照读**。

升级带来的好处：RW不用互相阻塞、同样也可以解决上述RW所有安全问题。（传统是利用加锁解决）

**注意：**MVCC解决的仅是RW问题，因此WW更新丢失的问题仍存在，**WW由其他机制解决**，而不归MVCC管辖。

**MVCC实现原理**

主要是依赖：
1.  3个隐式字段
    1.  DB_TRX_ID
    2.  DB_ROLL_PTR
    3.  DB_ROW_ID
1.  undo日志

1.  Read View
