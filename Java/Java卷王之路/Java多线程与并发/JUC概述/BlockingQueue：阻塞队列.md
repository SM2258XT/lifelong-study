---
title: BlockingQueue：阻塞队列
updated: 2022-09-10T17:40:11
created: 2022-09-10T17:02:31
---

**BlockingQueue是一个父接口，常见实现类有：**
1.  **ArrayBlockingQueue：定长、有界** 阻塞队列
2.  **LinkedBlockingQueue：可定长、有界** 阻塞队列
由链表实现，但大小默认值为Interger.Max_Value，可以手动指定。
1.  **DelayQueue：优先级 延迟 无界** 阻塞队列
支持优先级排序
1.  **SynchronousQueue：单元素** 队列
不存储元素
1.  **LinkedTransferQueue：无界** 阻塞队列
2.  **LinkedBlockingDeque：双向** 阻塞队列
**常用方法：**
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
四个等级按照出错时的处理情况划分，分别对应：
报错 \> 返回特殊值 \> 阻塞直至成功 \> 阻塞直至成功或超时
