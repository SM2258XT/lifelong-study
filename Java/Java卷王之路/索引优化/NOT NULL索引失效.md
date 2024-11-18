---
title: NOT NULL索引失效
updated: 2022-10-23T10:12:56
created: 2022-10-18T10:58:44
---

**IS NULL走索引**：NULL也算是一种特殊值（可以理解为0），所以B+Tree会对NULL进行排序
**IS NOT NULL索引失效：**属于范围查询（可以理解为 != 0 或 != NULL）

**字段尽可能用NOT NULL，而不是NULL，除非有特殊情况！**
NULL列在行中需要额外的空间以记录其值是否为NULL。 对于MyISAM表，每个NULL列都多花一位，四舍五入到最接近的字节。
