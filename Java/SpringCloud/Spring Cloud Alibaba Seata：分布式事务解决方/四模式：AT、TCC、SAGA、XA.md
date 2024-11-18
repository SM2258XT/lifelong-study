---
title: 四模式：AT、TCC、SAGA、XA
updated: 2022-10-19T11:13:27
created: 2022-10-19T09:31:28
---

四模式：AT、TCC、SAGA、XA
2022年10月19日
9:31

**分布式事务的底层服务调用实现**
Seata内置了对以下两种方式的支持
1.  Dubbo发布并调用
2.  SpringMVC提供API，Feign调用

**四种事务模式**
1.  AT
用的最多
1.  TCC
2.  Saga
3.  XA

**AT模式详解**
- 通过反向生成补偿SQL，实现数据回滚。但这些补偿SQL的保存，需要存储在数据库中额外创建的一张UNDO_LOG表里。
例如：

业务语句：insert into 订单 values（101,…）

seata会反向生成：delete from 订单 where id = 101
- TC协调全局事务提交后，UNDO_LOG会删除。若出错回滚时，则自动从UNDO_LOG中，提取补偿SQL并执行。

**AT模式优缺点总结**
优点：使用简单、性能好
缺点：
1.  属于分布式AP模式，存在数据不一致情况。
2.  要求对于所有数据库都具备管辖权，因为必须创建UNDO_LOG表。在涉及第三方时不一定可用（总不能修改支付宝的数据库吧）
