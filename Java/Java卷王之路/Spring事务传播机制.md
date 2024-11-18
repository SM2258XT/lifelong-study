---
title: Spring事务传播机制
updated: 2022-11-10T15:04:14
created: 2022-11-10T14:56:18
---

1.  Required：有则加入，无则新建
2.  Support：若存在事务则加入，无则非事务运行
3.  Mandatory：存在则加入，否则抛异常
4.  Requires_new：总是新建事务，若之前存在事务，则将之前的挂起。回滚也是只回滚自己的事务
5.  Not Support：自己以非事务方式运行，若之前有事务则挂起
6.  Never：AB都不能有事务，否则抛异常
7.  Nested：嵌套事务，父子事务
