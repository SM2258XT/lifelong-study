---
title: redolog：重做日志
updated: 2022-11-02T14:46:27
created: 2022-11-02T13:35:06
---

**InnoDB独有的崩溃恢复日志**

读取：先读Buffer Pool，没有再IO并更新Pool

写入：先写redo log buffer，之后以一定的频率，刷写到page cache（由操作系统控制）

**缓存刷盘频率控制**
1.  **事务提交触发刷盘**
InnoDB提供了3种策略，使用innodb_flush_log_at_trx_commit = 0 1 2控制

0）每次事务提交不刷，坐等系统后台定时刷

1）总会刷写并同步（默认）

2）总会刷写但是只写到page cache（由OS自行决定同步时机）
1.  **后台线程定时刷盘**
每隔1秒刷盘并同步。因此即使事务还没提交，也可能被刷盘
1.  **buffer占用过半触发刷盘**
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
