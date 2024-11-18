---
title: Channel、Exchange、Queue理解
updated: 2022-10-03T15:56:07
created: 2022-10-03T15:47:36
---

queue 具有自己的 erlang 进程；
- 可存放的message数量无限制
exchange 内部实现为保存 binding 关系的**查找表**；
channel 是实际进行路由工作的实体，即负责按照 routing_key 将 message 投递给 queue 。
- 每一个 channel 运行在一个独立的线程上，多线程共享同一个 socket
- 由于 TCP 连接的创建和销毁开销较大，且并发数受系统资源限制，会造成性能瓶颈。RabbitMQ 使用channel的方式来传输数据。信道是建立在真实的 TCP 连接内的虚拟连接，且每条 TCP连接上的channel数量没有限制。

**单 node 和多 node 构成的 cluster 系统中，声明 queue、exchange ，以及进行 binding 会有什么不同？**
多node必须等所有元数据更新完了，才会收到 Queue.Declare-ok 回应。（其实单node同理，因为只有单个节点的元数据）
另外，若 node 类型为 RAM node 则变更的数据仅保存在内存中，若类型为 Disk node 则还要变更保存在磁盘上的数据。

