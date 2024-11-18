---
title: 自增锁：关于AutoIncrement
updated: 2022-10-17T11:06:21
created: 2022-10-17T10:13:40
---

**概念：**
当向使用含有AUTO_INCREMENT列的表中插入数据时，需要获取的一种特殊的表级锁
在执行插入语句时就在**表级别**加一个**AUTO-INC**锁，然后为每条待插入记录的AUTO_INCREMENT修饰的列分配递增的值，在该语句执行结束后，再把AUTO-INC锁释放掉。

一个事务在持有AUTO-INC锁的过程中，其他事务的插入语句都要被阻塞，可以保证一个语句中分配的递增值是连续的。也正因为此，其并发性显然并不高。
innodb通过innodb_autoinc_lock_mode的不同取值来提供不同的锁定机制，来显著提高SQL语句的可伸缩性和性能。

**innodb_autoinc_lock_mode三种取值**，分别对应与不同锁定模式：
（1）innodb_autoinc_lock_mode = 0(“传统”锁定模式)
在此锁定模式下，所有类型的insert语句都会获得一个特殊的表级AUTO-INC锁，用于插入具有AUTO_INCREMENT列的表。这种模式其实就如我们上面的例子，即每当执行insert的时候，都会得到一个表级锁(AUTO-INC锁)，使得语句中生成的auto_increment为顺序，且在binlog中重放的时候，可以保证master与slave中数据的auto_increment是相同的。因为是表级锁，当在同一时间多个事务中执行insert的时候，对于AUTO-INC锁的争夺会\`限制并发能力\`。
（2）innodb_autoinc_lock_mode = 1(“连续”锁定模式)
在 MySQL 8.0 之前，连续锁定模式是\`默认\`的。

在这个模式下，“bulk inserts”仍然使用AUTO-INC表级锁，并保持到语句结束。这适用于所有INSERT ...

SELECT，REPLACE ... SELECT和LOAD DATA语句。同一时刻只有一个语句可以持有AUTO-INC锁。

对于“Simple inserts”（要插入的行数事先已知），则通过在\`mutex（轻量锁）\`的控制下获得所需数量的

自动递增值来避免表级AUTO-INC锁， 它只在分配过程的持续时间内保持，而不是直到语句完成。不使用

表级AUTO-INC锁，除非AUTO-INC锁由另一个事务保持。如果另一个事务保持AUTO-INC锁，则“Simple

inserts”等待AUTO-INC锁，如同它是一个“bulk inserts”。
（ 3 ）innodb_autoinc_lock_mode = 2(“交错”锁定模式)
从MySQL 8.0 开始，交错锁模式是\`默认\`设置。

在这种锁定模式下，所有类INSERT语句都不会使用表级AUTO-INC锁，并且可以同时执行多个语句。这是最快和最可扩展的锁定模式，但是当使用基于语句的复制或恢复方案时，\`从二进制日志重播SQL语句时，这是不安全的。(主从复制id可能不一致)\`

在此锁定模式下，自动递增值\`保证\`在所有并发执行的所有类型的insert语句中是唯一且单调递增的。但是，由于多个语句可以同时生成数字（即，跨语句交叉编号）， 为任何给定语句插入的行生成的值可能不是连续的。

