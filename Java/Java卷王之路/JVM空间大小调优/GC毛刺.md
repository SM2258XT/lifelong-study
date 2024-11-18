---
title: GC毛刺
updated: 2022-11-22T07:56:21
created: 2022-11-22T07:32:16
---

GC毛刺
2022年11月22日
7:32

**高峰期GC导致服务可用性降低**
GC日志显示，高峰期CMS在重标记（**Remark**）阶段耗时1.39s。Remark阶段是**STW**的，在执行垃圾回收时，Java应用程序中除了垃圾回收器线程之外其他所有线程都被挂起，意味着在此期间，用户正常工作的线程全部被暂停下来，这是低延时服务不能接受的。本次优化目标是降低Remark时间。

**新生代GC和老年代GC是各自独立分开进行的**
由于只有MinorGC才会使用根搜索算法，来标记新生代对象是否可达。（也就是说虽然某些对象已经不可达，但在MinorGC前，不会被标记为不可达）
所以CMS无法判断哪些对象不可达，Remark时只能整堆扫描
所以整堆中的对象数目影响了Remark耗时！！！
分析GC日志也可以得到同样规律：Remark\>500ms时，新生代使用率\>75%
**结论1：**降低Remark阶段耗时问题，转换为了减少新生代对象数量

**可中断的并发预清理**
新生代对象数量最大特点就是朝生夕死，如果Remark前执行一次MinorGC，则可以有效回收大部分对象。CMS就采用了这种方式。
CMS在Remark前，增加了一个可中断的并发预清理（CMS-concurrent-abortable-preclean），主要工作就是并发标记存活对象，只不过是可中断的。
该阶段在Eden \> 2m时启动，如果此阶段执行时等待了MinorGC，则刚好就将上述标记对象回收，这样Remark阶段需要扫描的对象就少了很多。
预清理不会无限等待MinorGC，CMSMaxAbortablePrecleanTime默认为5秒。到时候无论怎样都会终止等待，直接Remark.
可以使用CMSScavengeBeforeRemark，在Remark前强制执行一次MinorGC

**优化结果**
经过增加CMSScavengeBeforeRemark参数，单次执行时间\>200ms的GC停顿消失，从监控上观察，GCtime和业务波动保持一致，不再有明显的毛刺。

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
