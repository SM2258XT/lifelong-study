---
title: HashTable概述
updated: 2022-09-15T14:50:40
created: 2022-09-15T14:33:02
---

**背景：为了解决Hashmap线程不安全的问题而诞生**
但是仅仅只是非常暴力的在方法上加synchronized，**性能损失比较大**，几乎不用了。（远古时代JDK1.0产物，就连Hashmap都是1.2才有的）
替代品：JUC下的ConcurrentHashmap

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
