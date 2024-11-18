---
title: 自动注入ByType
updated: 2021-03-09T19:05:40
created: 2021-03-09T18:55:49
---

autowire = "byType"

在当前xml中找，找某个bean的类 与 自动注入bean缺省属性的类。存在同源关系。

同源关系：同一类、父子类、接口实现关系

注意：
1.  如果用ByType注入，那么需要的bean就必须具有xml范围的唯一性！！
