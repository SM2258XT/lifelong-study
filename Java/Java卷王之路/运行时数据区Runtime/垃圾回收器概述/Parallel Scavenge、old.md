---
title: Parallel Scavenge、old
updated: 2023-02-26T10:40:20
created: 2022-09-15T16:00:23
---

Parallel Scavenge、old
自适应调节策略
2022年9月15日
16:00

**Parallel Scavenge / Parallel Old 回收器：吞吐量优先**
- 在原ParNew的基础上，实现了**可控制的吞吐量**。
- 自适应调节策略是与PaeNew最本质的区别

**特点：**
STW、并行、自适应调节（可控吞吐量）

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
