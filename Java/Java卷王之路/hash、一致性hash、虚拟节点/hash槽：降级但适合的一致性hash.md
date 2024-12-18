---
title: hash槽：降级但适合的一致性hash
updated: 2022-10-22T09:30:09
created: 2022-10-15T09:37:03
---

hash槽：降级但适合的一致性hash
<https://blog.csdn.net/qq_44833552/article/details/123997903>
2022年10月15日
9:37

**什么是hash slot**
一个 Redis Cluster包含16384（0~16383）个哈希槽（可以假设成盒子），存储在Redis Cluster中的所有键都会被映射到这些slot中，集群中的每个键都属于这16384个哈希槽中的一个。按照槽来进行分片，通过为每个节点指派不同数量的槽，可以控制不同节点负责的数据量和请求数
eg：当前集群有3个节点,槽默认是平均分的:

节点 A （6381）包含 0 到 5499号哈希槽.
节点 B （6382）包含5500 到 10999 号哈希槽.
节点 C （6383）包含11000 到 16383号哈希槽
增加一个 master节点，就将其他 master 的 hash slot 移动部分过去
减少一个 master节点，就将它的 hash slot 移动到其他 master 上去
这样移动 hash slot 的成本是非常低的，并且将一个哈希槽从一个节点移动到另一个节点不会造成节点阻塞， 所以无论是添加新节点还是移除已存在节点， 又或者改变某个节点包含的哈希槽数量， 都不会造成集群下线
那hash slot是怎么分槽的呢？
集群使用公式slot=CRC16（key）/16384来计算key属于哪个槽，其中CRC16(key)语句用于计算key的CRC16 校验和

**Redis Cluster的之所以使用Hash Slot 的16384(214)，而不使用一致性hash的216**
Redis的作者认为CRC16(key) mod 16384的效果已经不错了，虽然没有一致性hash灵活，但实现很简单，节点增删时处理起来也很方便
redis的一个节点的心跳信息中需要携带该节点的所有配置信息，而16K(214)大小的槽数量所需要耗费的内存为2K(2 \* 8bit \*1K=16K)，但如果使用65K(216)个槽，这部分空间将达到8K(8 \* 8bit \*1K=65K)，心跳信息就会很庞大
你可能认为2k和8k差不多，但当节点非常多时，4倍差距将会放的很大，当然Redis集群中主节点的数量基本不可能超过1000个
Redis主节点的配置信息中，它所负责的哈希槽是通过一张bitmap的形式来保存的，在传输过程中，会对bitmap进行char压缩，但是如果bitmap的填充率slots / N很高的话，bitmap的压缩率就很低，所以N表示节点数，如果节点数很少，而哈希槽数量很多的话，bitmap的压缩率就很低。而16K个槽且主节点为1000的时候，是刚好比较合理的，既保证了每个节点有足够的哈希槽，又可以很好的利用bitmap
选取了16384是因为CRC16会输出16bit的结果，可以看作是一个分布在0-216-1之间的数，redis的作者测试发现这个数对214求模的会将key在0-214-1之间分布得很均匀，因此选了这个值

**一致性hash与hash slot区别**
通过上面的分析，不难发现两种都是解决数据与节点之间的映射，简而言之，就是致力于斩断数据与节点之间的联系，虽然两者都实现了，但在数据分布均匀方面，hash slot比一致性hash显得更加均匀，因为slot数的选择一方面依赖于CRC16校验的16bit key值对214求模的会将key在0-214-1之间分布得很均匀，同时一致性hash是根据hash函数计算的值，在hash环上随机；

**一致性哈希–环的数据结构保存在哪里**
中央协调点：使用专用机器保持一个环并充当中央负载平衡器，将请求路由到适当的节点。优点：非常简单的实现。这将非常适合大量节点（数据）的动态系统。缺点：可扩展性和可靠性差。稳定的分布式系统没有单一的故障。
没有协调的中心点——完全复制：每个节点都保留一个完整的环副本。适用于稳定的网络。此选项用于例如 Amazon Dynamo。优点：查询直接路由到适当的缓存服务器。缺点：加入(离开)环中的服务器需要通知环中的所有缓存服务器。
有协调的中心点——部分复制：每个节点保留环的部分副本。这个选项是CHORD算法的直接实现。就 DHT（分布式哈希表）而言，每个缓存机器都有其前置和后继，并且在接收到查询时检查它是否具有密钥。如果那台机器上没有这样的密钥，则使用映射函数来确定其邻居（后继和前任）中的哪个与该密钥的距离最小。然后它将查询转发给距离最小的邻居。该过程一直持续到当前缓存机器找到密钥并将其发回。优点：对于高度动态的更改，由于节点之间的闲聊开销很大，因此前一个选项不合适。因此，此选项是这种情况下的选择。缺点：没有消息的直接路由。

