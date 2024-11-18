---
title: Spring Cloud Alibaba Seata：分布式事务解决方案
updated: 2022-02-02T18:47:02
created: 2022-02-02T16:35:11
---

单体应用被拆分成微服务应用，原来的三个模块被拆分成三个独立的应用，分别使用三个独立的数据源，业务操作需要调用三三 个服务来完成。此时每个服务内部的数据一致性由本地事务来保证， 但是全局的数据一致性问题没法保证。
Seata 是一款开源的分布式事务解决方案，致力于提供高性能和简单易用的分布式事务服务。Seata 将为用户提供了 AT、TCC、SAGA 和 XA 事务模式，为用户打造一站式的分布式解决方案。

分布式事务处理过程的一ID + 三组件模型：

一ID：
Transaction ID XID 全局唯一的事务ID
三组件：
TC (Transaction Coordinator) - 事务协调者：维护全局和分支事务的状态，驱动全局事务提交或回滚。

TM (Transaction Manager) - 事务管理器：定义全局事务的范围：开始全局事务、提交或回滚全局事务。

RM (Resource Manager) - 资源管理器：管理分支事务处理的资源，与TC交谈以注册分支事务和报告分支事务的状态，并驱动分支事务提交或回滚。

处理过程：
TM向TC申请开启一个全局事务，全局事务创建成功并生成一个全局唯一的XID；

XID在微服务调用链路的上下文中传播；

RM向TC注册分支事务，将其纳入XID对应全局事务的管辖；

TM向TC发起针对XID的全局提交或回滚决议；

TC调度XID下管辖的全部分支事务完成提交或回滚请求。

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
