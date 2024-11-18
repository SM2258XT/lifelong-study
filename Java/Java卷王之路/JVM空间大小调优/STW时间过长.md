---
title: STW时间过长
updated: 2022-11-22T08:10:19
created: 2022-11-22T07:55:07
---

STW时间过长
<https://tech.meituan.com/2017/12/29/jvm-optimize.html>
2022年11月22日
7:55

**STW时间过长**
GC日志如下图（在GC日志中，Full GC是用来说明这次垃圾回收的停顿类型，代表STW类型的GC，并不特指老年代GC），根据GC日志可知本次Full GC耗时1.23s。
这个在线服务同样要求低时延高可用。本次优化目标是降低单次STW回收停顿时间，提高可用性。

**Full GC触发原因**
1.  Perm空间不足
2.  CMS GC时出现promotion failed和concurrent mode failure
一般是CMS正在进行，但是由于老年代空间不足，需要尽快回收老年代里面的不再被使用的对象，这时停止所有的线程，同时终止CMS，直接进行**Serial Old GC**
1.  统计得到的Young GC晋升到老年代的平均大小大于老年代的剩余空间
2.  主动触发Full GC
执行 jmap -histo:live \[pid\] 来避免碎片问题。

**Perm空间不足扩容导致FullGC**
根据日志发现Full GC后，Perm区变大了，推断是由于永久代空间不足容量扩展导致的
以下是可用解决方案，根据具体情况选择：
1.  通过把-XX:PermSize参数和-XX:MaxPermSize设置成一样，强制虚拟机在启动的时候就把永久代的容量固定下来，避免运行时自动扩容。
2.  CMS默认情况下不会回收Perm区，通过参数CMSPermGenSweepingEnabled、CMSClassUnloadingEnabled ，可以让CMS在Perm区容量不足时对其回收。
由于该服务**没有生成大量动态类，回收Perm区收益不大**，所以我们采用方案1，启动时将Perm区大小固定，避免进行动态扩容。

**总结**
对于性能要求很高的服务，建议将MaxPermSize和MinPermSize设置成一致（JDK8开始，Perm区完全消失，转而使用元空间）
将Xms和Xmx也设置为相同，这样可以减少内存自动扩容和收缩带来的性能损失。
虚拟机启动的时候就会把参数中所设定的内存全部化为私有，即使扩容前有一部分内存不会被用户代码用到，这部分内存在虚拟机中被标识为虚拟内存，也不会交给其他进程使用。
