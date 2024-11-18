---
title: 隐式字段 和 undo log
updated: 2022-10-17T16:19:04
created: 2022-10-17T14:50:00
---

**三个隐式字段**

每行记录除了我们自定义的字段外，还有数据库隐式定义的字段：
1.  **DB_TRX_ID**：最近（insert/update）事务ID
记录创建这条记录/最后一次修改该记录的事务ID
1.  **DB_ROLL_PTR**：回滚指针
用于配合undo日志，指向这条记录的上一个版本（存储于rollback segment里）
1.  **DB_ROW_ID**：隐藏主键
如果数据表没有主键，InnoDB会自动以DB_ROW_ID产生一个聚簇索引，隐式自增主键。

实际还有一个删除flag隐藏字段, 既记录被更新或删除并不代表真的删除，而是删除flag变了

**undo日志**

undo log是一个有序的历史版本单向链表
1.  insert undo log
只在回滚时需要，一旦事务提交后就可以立即丢弃。用处不大。

insert和快照读没丁点关系，所以才用处不大（因为快照读，首先肯定是能读啊，insert之前都没有数据，读个铲铲）
1.  **update undo log**
不仅是update，还包括delete时产生的日志，也就是**update + delete**

**事务回滚**和**快照读**都需要，所以不能随便删除，只有在前两者都不涉及时，被purge线程统一清除。这个才是真正有用的MVCC依赖日志。
