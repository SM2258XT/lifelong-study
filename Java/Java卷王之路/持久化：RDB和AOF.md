---
title: 持久化：RDB和AOF
updated: 2022-11-21T18:50:31
created: 2022-05-13T21:31:58
---

Redis 是内存数据库，如果不将内存中的数据库状态保存到磁盘，那么一旦服务器进程退出，服务器中的数据库状态也会消失。所以Redis提供了持久化功能!
1.  **RDB（Redis DataBase），更应该叫做snapshot**
    - **Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。**
    - **极高的性能：整个过程中，主进程是不进行任何IO操作的。**
    - **如果需要进行大规模数据的恢复（因为只记录了数据，规模本来就很大了，就不用AOF再去记录每一步操作），且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。**
    - **缺点：最后一次持久化后的数据可能丢失。我们默认的就是RDB，一般情况下不需要修改这个配置！**
2.  **AOF（Append Only File）**
流程：
1.  所有的写入命令，都会追加到一个缓冲区aof_buf中
2.  aof_buf 定期同步到磁盘
3.  AOF文件越来越大时，需要定期整理rewrite以压缩
4.  重启redis后，加载AOF文件以恢复
优点：

1、每一次修改都同步，文件的完整性会更加好！

2、每秒同步一次，最多会丢失一秒的数据！

3、从不同步，效率最高的！

缺点：

1、相对于数据文件来说，aof远远大于 rdb，修复的速度也比 rdb慢！

2、Aof 运行效率也要比 rdb 慢，所以我们redis默认的配置就是rdb持久化！

**总结：以下来自伟大的网友总结！**

1、RDB 持久化方式能够在指定的时间间隔内对你的数据进行快照存储

2、AOF 持久化方式记录每次对服务器写的操作，当服务器重启的时候会重新执行这些命令来恢复原始的数据，AOF命令以Redis 协议追加保存每次写的操作到文件末尾，Redis还能对AOF文件进行后台重写，使得AOF文件的体积不至于过大。

3、只做缓存，如果你只希望你的数据在服务器运行的时候存在，你也可以不使用任何持久化

4、**同时开启两种持久化方式**

在这种情况下，当redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整。

RDB 的数据不实时，同时使用两者时服务器重启也只会找AOF文件，那要不要只使用AOF呢？建议不要，因为RDB更适合用于备份数据库（AOF在不断变化不好备份），快速重启，而且不会有AOF可能潜在的Bug，留着作为一个万一的手段。

**性能建议**

因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留 save 900 1 这条规则。

如果Enable AOF ，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了，代价一是带来了持续的IO，二是AOF rewrite 的最后将 rewrite 过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。只要硬盘许可，应该尽量减少AOF rewrite的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上，默认超过原大小100%大小重写可以改到适当的数值。

如果不Enable AOF ，仅靠 Master-Slave Repllcation 实现高可用性也可以，能省掉一大笔IO，也减少了rewrite时带来的系统波动。代价是如果Master/Slave 同时倒掉，会丢失十几分钟的数据，启动脚本也要比较两个 Master/Slave 中的 RDB文件，载入较新的那个，微博就是这种架构。
