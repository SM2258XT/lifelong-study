---
title: 三范式、char(11)等
updated: 2022-11-13T14:24:51
created: 2022-11-13T13:56:56
---

实际开发中，不会遵循三范式

金钱相关数据类型选择：int、bigint，或者使用decimal（对应BigDecimal类）

varchar(50) 中的 50 代表涵义
varchar(50)和(2000)存储"hello"所占空间一样大！只是后者在排序时
