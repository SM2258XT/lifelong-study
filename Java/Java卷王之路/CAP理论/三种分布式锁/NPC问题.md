---
title: NPC问题
updated: 2022-10-27T10:00:51
created: 2022-10-25T16:48:38
---

NPC问题
扩展：Google的Chubby分布式锁
2022年10月25日
16:48

**NPC问题（Network Delay、Process Pause(GC)、Clock Drift）**
- 网络延迟
- 处理器暂停
- 时钟飘逸

**故障重启带来的问题**
假设一共有 A、B、C 这三个节点。
1.  客户端 1 在 A，B 上加锁成功。C 上加锁失败。
2.  这时节点 B 崩溃重启了，但是由于持久化策略导致客户端 1 在 B 上的锁没有持久化下来。
3.  客户端 2 发起申请同一把锁的操作，在 B，C 上加锁成功。
4.  这个时候就又出现同一把锁，同时被客户端 1 和客户端 2 所持有了。
时钟飘逸同理，本质上仍然是某个**有锁的节点**，因为重启丢失数据，或时钟飘逸提前过期，**变成了无锁的节点**，而刚好又可以**与剩下无锁节点组成50%+**

**广义的STW**
不只是Java的垃圾回收STW，泛指以下等例子：
1.  VM环境，暂停VM、备份VM到磁盘、迁移VM等
2.  用户终端设备，例如笔记本合上盖子休眠等
3.  操作系统上下文切换，被其他程序中断，CPU时钟窃取，进程饥饿等
4.  IO操作等
5.  配置了SWAP而产生缺页中断，必须进行IO等
这也解释了为什么服务器通常禁用SWAP，宁愿杀死一些进程来释放内存，而不是反复抖动
1.  SIGSTOP-SIGCONT、CtrlZ等信号

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)
