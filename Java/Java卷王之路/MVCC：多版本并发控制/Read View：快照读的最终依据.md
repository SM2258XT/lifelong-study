---
title: Read View：快照读的最终依据
updated: 2024-09-11T15:10:39
created: 2022-10-17T15:23:10
---

**执行update/delete时，顺序如下**
假设原始行数据为
1.  加行锁
再次提醒，这里加行锁是为了解决WW冲突，而RW冲突直接快照读，虽然行锁存在但是没有使用。
1.  拷贝原始数据到undo log链表中作为副本，插入到表头位置。
2.  在行记录中（而不是undo log中），修改TRX_ID为当前事务，修改ROLL_PTR指向undo log中的副本。
3.  提交事务，释放行锁。
根据以上流程，可以看出undo log顺序链表，最新的历史记录总在表头。

**Read View读视图**
更像是一套判断依据。通过这些原始的判断依据，用来判断当前事务能够看到undo log中哪个版本的数据。
也就是说Read View更像是if - else括号里的判断条件，而不是当前或历史数据。

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)

<https://www.bilibili.com/video/BV1hL411479T>
