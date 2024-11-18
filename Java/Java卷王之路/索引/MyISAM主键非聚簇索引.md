---
title: MyISAM主键非聚簇索引
updated: 2022-10-12T10:26:38
created: 2022-10-12T10:21:37
---

- MyISAM 的主键索引使用的是非聚簇索引，索引文件和数据文件是分离的，连主键索引文件都仅保存数据的地址。
- 主键索引 B+ 树的节点存储了主键，辅助键索引 B+ 树存储了辅助键，表数据存储在独立的地方，这两颗 B+ 树的叶子节点都使用一个**地址**指向真正的表数据，对于表数据来说，这两个键没有任何差别
- 由于索引树是独立的，通过辅助索引检索无需回表查询访问主键的索引树

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)
