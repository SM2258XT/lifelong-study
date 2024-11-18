---
title: SpringBoot堆外内存泄露总结
updated: 2022-11-22T14:16:03
created: 2022-11-22T13:34:25
---

SpringBoot堆外内存泄露总结
<https://tech.meituan.com/2019/01/03/spring-boot-native-memory-leak.html>
2022年11月22日
13:34

使用top等指令查看：配置了4G堆内内存，但是实际使用的物理内存竟然高达7G（-Xms4g -Xmx4g）

**排查过程**
1.  **使用Java层面的工具定位内存区域**
主要是定位哪个区域的内存泄漏了：堆？堆外？Code区？

第一步：-XX:NativeMemoryTracking=detail ，重启项目

第二步：jcmd pid VM.native_memory detail 查看内存分布

jcmd显示的内存包括：堆内存、Code区、Unsafe里正常申请的堆外内存（allocateMemory、DirectByteBuffer），但是**不包含**其他C代码（Native Code）申请的堆外内存
1.  此时发现committed的内存**小于物理内存**（比较的不是Xmx最大值哈！！！），但使用top却发现物理内存已占满，说明很可能是其他Native Code内存泄漏导致的
2.  再使用pmap -x pid \| sort -k 3 -n -r查看内存分布，发现大量64M地址，而这些地址空间不在jcmd命令所给出的地址空间内

1.  **使用系统层面工具，定位堆外泄漏内存**
已经基本上确定是Native Code所引起，而Java层面的工具不便于排查此类问题，只能使用系统层面的工具去定位问题。
1.  使用gperftools

后面看不懂了……
