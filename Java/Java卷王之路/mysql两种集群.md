---
title: mysql两种集群
updated: 2022-11-02T16:06:20
created: 2022-10-14T10:32:53
---

mysql两种集群
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
2022年10月14日
10:32

1.  **单库设计**
无集群
1.  **主从读写分离集群**
分片中间件作用：读写分离。将读请求和写请求，路由到slaver或master。可用MyCat、ShardingSphere等。

binlog日志同步：mysql自带功能，用于slaver从master同步数据。

特点：每个节点存储的都是全量数据。适用于100w+规模应用。
1.  **分库分表（Sharding）**
分片中间件作用：将请求数据分片并路由到其中一个node。可用MyCat、ShardingSphere等。

特点：
1.  每个节点只存储一部分数据。适用于十亿级最大规模应用。
2.  容灾性较低。
分片算法：
1.  范围法。简单，但数据分布不均匀。
2.  Hash法。分布均匀，但数据扩展迁移难度大（因为原始数据必须全部再hash一遍）。

**读写分离+分片组合（互联网最常用）**

分库分表的基础上，再将每个节点视为写master，为其分配多个读slaver。

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image3.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image4.png)
