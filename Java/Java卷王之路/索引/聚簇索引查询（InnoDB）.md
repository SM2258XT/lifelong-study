---
title: 聚簇索引查询（InnoDB）
updated: 2022-10-12T10:21:28
created: 2022-10-12T09:44:23
---

聚簇索引查询（InnoDB）
2022年10月12日
9:44

**示例准备：**
CREATE TABLE \`user_innodb\`
(
\`id\` int(11) NOT NULL AUTO_INCREMENT,
\`username\` varchar(20) DEFAULT NULL,
\`age\` int(11) DEFAULT NULL,
PRIMARY KEY (\`id\`) USING BTREE,
KEY \`idx_age\` (\`age\`) USING BTREE
) ENGINE = InnoDB;
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)

**聚簇索引，直接查到：**
**辅助索引，需要回表：**

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image3.png)
