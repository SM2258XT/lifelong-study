---
title: Redis - SET NX EX
updated: 2022-10-27T10:38:34
created: 2022-05-15T16:27:46
---

在Redis单条命令式保证原子性，但是事务不保证原子性！事务出错**不能回滚**，出错时只会，之前正确的不会撤销！

在Redis事务没有没有隔离级别的概念（我的理解：因为根本就没有并发的事务，都是串行执行的）

Redis事务功能是通过MULTI、EXEC、DISCARD和WATCH四个原语实现的

其中，WATCH命令可以提供CAS行为，监控一个或多个键，一旦被修改，之后的事务就不会执行（但是之前成功的不会撤销）。

Redis本身没有锁的概念，只是后期使用技巧来实现锁的效果！

**Redis实现分布式锁**：使用**SETNX**命令

redis补充：
1.  redis从2.6.12版本开始，为SET命令增加了一个命令SETNX（NotExist），只有**不存在时才能set成功**。
每个服务发现缓存没有，都想去查数据库时，先向redis进行set nx，如果成功了就表示自己占据住了分布式锁，可以继续查数据库。
1.  SET EX：设置过期时间，例如：set aaa 11 EX 500（设置aaa的值为11，并指定500秒过期），可以使用ttl \[key\]来查看剩余时间
简单测试SET NX：

创建多个同一redis的连接（xshell开多个窗口连接虚拟机），批量进入redis-cli

测试set nx，批量输入set mykey1 hahaha NX执行

结果：只有其中一个客户端连接返回的是ok，其他返回的都是nil

**分布式锁（客户端）**

对连接进行池化，同时对客户端读写Redis操作采用内部锁synchronized。

**分布式锁（服务端）**

该操作对应java里的：redisTemplate.setIfAbsent（k , v），但是有许多隐患：
1.  **阶段1隐患：无过期时间**
使用setnx的业务逻辑代码应该是自旋的（又不是JAVA，没法阻塞啊），如果没有获取到锁则等待一段时间后重试，直至获取到锁并执行完业务后删除锁。也就是必须有锁才进行下一步。

但是如果setnx占有了锁，执行过程中发生了异常宕机没有释放锁，当前业务代码就永远无法成功SETNX，最终会导致分布式死锁。

解决：设置锁的自动过期时间redisTemplate.expire（）
1.  **阶段2隐患：过期时间没设置成功**
由于Redis只有单条指令是原子操作，如果在成功设置锁之后，继续设置过期时间的时候刚好宕机，则redis中的锁相当于还是永远释放不了，导致死锁

解决：加锁和过期**作为原子命令**同时执行：redisTemplate.opsForValue().setIfAbsent（key,value,timeout,timeuint）

注意：必须使用4个参数的重载方法，底层是set key value \[EX seconds\] \[PX milliseconds\] \[NX\|XX\] ，是原子性的。其他方法会拆成两条命令执行。

对应于指令：set aaa 11 EX 500 NX
1.  **阶段3隐患：释放别人的锁**
如果业务代码超长，在还未执行完时，锁A就过期删除了，而其他业务发现锁A不存在没有人占锁，于是继续往A设置锁。导致最初超长的业务执行完时，可能删除的是其他业务的锁。（有点类似于ABA问题）

解决：使用**UUID或时间戳**作为锁的value，每次删除锁之前先获取一下value并查看是否和之前的uuid一致。（UUID不应该作为KEY！！）

但是获取值+对比成功删除 也应该是原子操作，而这种原子操作只能依靠lua脚本实现

redisTemplate.excute(new DefaultRedisScript())
1.  **阶段4隐患：Redis集群的最终一致性复制，导致锁不一定复制成功**
Redis为异步复制（AP模型），Slaver还没同步完锁，Master就挂了，那么这个锁就还可以被再次获取。
