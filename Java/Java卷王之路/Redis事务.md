---
title: Redis事务
updated: 2022-10-15T14:37:15
created: 2022-10-15T08:42:52
---

Redis事务
2022年10月15日
8:42

Redis事务没有隔离级别的概念！
Redis单条命令保证原子性，但是事务不保证原子性！
主要命令：multi - exec - discard、watch - unwatch（乐观锁）

事务的整体原子性实现：
1.  Lua脚本
2.  使用多个中间标记变量
