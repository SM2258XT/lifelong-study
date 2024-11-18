---
title: =====  mysql  =====
updated: 2022-10-14T16:34:32
created: 2022-05-10T19:22:32
---

1.  一般如何优化SQL？
加索引、避免返回不必要的数据、适当分批量进行、优化sql结构、主从架构，提升读性能、分库分表
1.  数据库引擎InnoDB与MyISAM的区别
    1.  InnoDB支持事务、外键、MVCC（多版本并发控制）
    2.  InnoDB支持表、行级锁
select count(\*) from table时，MyISAM更快，因为它有一个变量保存了整个表的总行数，可以直接读取，InnoDB就需要全表扫描。

Innodb不支持全文索引，而MyISAM支持全文索引（5.7以后的InnoDB也支持全文索引）

InnoDB支持表、行级锁，而MyISAM支持表级锁。

InnoDB表必须有主键，而MyISAM可以没有主键

Innodb表需要更多的内存和存储，而MyISAM可被压缩，存储空间较小，。

Innodb按主键大小有序插入，MyISAM记录插入顺序是，按记录插入顺序保存。

InnoDB 存储引擎提供了具有提交、回滚、崩溃恢复能力的事务安全，与 MyISAM 比 InnoDB 写的效率差一些，并且会占用更多的磁盘空间以保留数据和索引

InnoDB 属于索引组织表，使用共享表空间和多表空间储存数据。MyISAM用.frm、.MYD、.MTI来储存表定义，数据和索引。
