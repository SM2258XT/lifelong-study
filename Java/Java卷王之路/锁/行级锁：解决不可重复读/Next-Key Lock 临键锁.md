---
title: Next-Key Lock 临键锁
updated: 2022-11-22T16:07:58
created: 2022-10-17T12:14:29
---

**临键锁Next-Key Lock：**
有时候我们不仅想锁住间隙（开区间），还想锁住当前行记录（某个点），因此诞生了临键锁Next-Key Lock（左开右闭）
临键锁 = 左间隙锁 + 记录锁

**左开右闭原则：**
InnoDB 加锁的基本单位是 next-key lock，该锁是行锁和 gap lock 的组合（X or S 锁），但是加锁过程是分为间隙锁和行锁两段执行
可以保护当前记录和前面的间隙，遵循左开右闭原则，单纯的间隙锁是左开右开
假设有 10、11、13，那么可能的间隙锁包括：(负无穷,10\]、(10,11\]、(11,13\]、(13,正无穷)

**例1：**
假设表中只有101条记录：1,2,3 … 100,101
Select \* from emp where id \> 100 for update; // 查询范围 \> 100
不仅会对 id = 101 的记录加其他锁，还会对 id \> **101** 的“间隙“加上锁。

**几种间隙锁使用情况：**
一定要抓住SQL给定的条件/范围，涉及条数是不确定的！
1.  范围语句。
涉及条数明显不确定，随时可能insert（不包括delete，因为记录本来就存在，是其他锁锁定的），必须间隙锁。
1.  请求不存在的记录
如果使用相等条件请求给一个不存在的记录加锁，InnoDB也会使用间隙锁！

向右遍历时且最后一个值不满足等值条件的时候，next-key lock退化为间隙锁

**例2：**
假设已存在id：1,3,4
SET AUTOCOMMIT=0; -- C1、C2
UPDATE test_innodb_lock SET name='8888' WHERE id \< 4; -- C1 先执行，设置间隙锁 ( 1 , 2 \]
INSERT INTO test_innodb_lock VALUES(**2**,'200','2'); -- C2 后执行，尝试insert id = 2，被间隙锁阻塞，直至C1提交释放锁。

