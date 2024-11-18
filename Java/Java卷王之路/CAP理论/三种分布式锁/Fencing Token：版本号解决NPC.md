---
title: Fencing Token：版本号解决NPC
updated: 2022-10-27T10:10:12
created: 2022-10-27T09:48:37
---

Fencing Token：版本号解决NPC
神仙打架全过程： <https://blog.csdn.net/meser88/article/details/116645035>
2022年10月27日
9:48

**Fencing栅栏**
确保因为异常原因，导致锁过期的那个节点不能影响其他正常的部分，要实现这一目标，可以采用一直相当简单的 fencing
流程：
1.  每次服务端在授予锁时，还会同时返回一个 fencing 令牌，该令牌每次授予都会递增。
2.  客户端每次向存储系统发送写请求时，都必须包含所持有的 fencing 令牌。
3.  存储系统需要对令牌进行校验，发现如果已经处理过更高令牌的请求，则拒绝执行该请求。

**版本号机制类似Zookeeper**
当使用 ZK 做锁服务时，可以用事务标识 zxid 或节点版本 version 来充当 fencing 令牌，这两个都可以满足单调递增的要求。

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
