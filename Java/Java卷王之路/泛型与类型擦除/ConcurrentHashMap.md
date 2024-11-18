---
title: ConcurrentHashMap
updated: 2023-03-05T08:57:10
created: 2022-05-16T18:32:24
---

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
ConcurrentHashMap作为Concurrent一族，其有着高效地并发操作，相比Hashtable的笨重，ConcurrentHashMap则更胜一筹了。

核心思想：**CAS+Synchronized**

**窥见CAS和Unsafe**
//Unsafe mechanics
private static final sun.misc.**Unsafe** U;
private static final long SIZECTL;
private static final long TRANSFERINDEX;
……
其中，sizeCtl：
控制标识符，用来控制table初始化和扩容操作的，在不同的地方有不同的用途，其值也不同，所代表的含义也不同
- 负数代表正在进行初始化或扩容操作
- -1代表正在初始化
- -N 表示有N-1个线程正在进行扩容操作
- 正数或0代表hash表还没有被初始化，这个数值表示初始化或下一次进行扩容的大小

