---
title: CAS简介
updated: 2022-11-10T16:32:33
created: 2021-09-25T12:43:47
---

CAS（Compare And Swap）：比较并交换。
CAS与synchronize、lock这些锁机制的本质原理不同，CAS由CPU硬件实现，而java由软件逻辑实现。
CAS 操作包含三个操作数 —— 内存位置（V）、预期原值（A）和新值(B)。 如果内存位置的值与预期原值相匹配，那么处理器会自动将该位置值更新为新值 。否则，处理器不做任何操作。无论哪种情况，它都会在 CAS 指令之前返回该位置的值。（在 CAS 的一些特殊情况下将仅返回 CAS 是否成功，而不提取当前值。）CAS 有效地说明了“我认为位置 V 应该包含值 A；如果包含该值，则将 B 放到这个位置；否则，不要更改该位置，只告诉我这个位置现在的值即可。”

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
