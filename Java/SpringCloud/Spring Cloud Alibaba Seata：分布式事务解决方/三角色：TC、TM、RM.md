---
title: 三角色：TC、TM、RM
updated: 2022-10-19T11:24:28
created: 2022-02-02T19:59:27
---

1.  TC (Transaction Coordinator) - 事务协调者：维护全局和分支事务的状态，驱动全局事务提交或回滚。
2.  TM (Transaction Manager) - 事务管理器：定义全局事务的范围，开始全局事务、提交或回滚全局事务。
3.  RM ( Resource Manager ) - 资源管理器：管理分支事务处理的资源( Resource )，与 TC 交谈以注册分支事务和报告分支事务的状态，并驱动分支事务提交或回滚。
TC需要单独部署服务器，而TM和RM则在客户端中。

**三角色协调步骤：**
1.  TM请求TC，开启一个新的全局事务。TC同意后生成XID。
2.  RM请求TC，将本地事务，注册为全局事务的一个分支事务，这样就能通过XID关联。
3.  TM请求TC，告诉XID对应的全局事务，提交还是回滚。
4.  TC驱动所有RM，对自己本地的事务（全局事务分支）进行提交或回滚。

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
