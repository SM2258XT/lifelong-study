---
title: JVM各种参数
updated: 2023-03-06T08:31:32
created: 2022-11-24T08:15:11
---

JVM各种参数
<https://segmentfault.com/a/1190000020656202>
2022年11月24日
8:15

**共分为三类**
1.  【-】标准参数，所有jvm必须实现，且向后兼容
2.  【-X】非标准参数，默认实现这些参数，但是不保证所有jvm的实现都满足，且不保证向后兼容
3.  【-XX】非稳定参数，各个JVM实现不同，将来也可能随时取消

**信息类参数**
打印启动参数：java -XX:+PrintCommandLineFlags -version
打印GC日志：-XX:+PrintGCDetails -XX:+PrintGCDateStamps -Xloggc:/home/GCEASY/gc-%t.log
打印ClassLoader日志：-XX:+TraceClassLoading -XX:+TraceClassUnloading
OOM时的内存dump： -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/crashes/my-heap-dump.hprof
OOM时的执行脚本： -XX:OnOutOfMemoryError=/scripts/restart-myapp.sh
打印JIT时间：-XX:-CITime
方法被编译时，打印相关信息：-XX:-PrintCompilation
JMX：
-Djava.net.preferIPv4Stack=true

-Dcom.sun.management.jmxremote

-Djava.rmi.server.hostname={hostname}

-Dcom.sun.management.jmxremote.port={port}

-Dcom.sun.management.jmxremote.authenticate=false

**内存类参数**
JVM配置了多少内存并不是说启动后就会占用多少物理内存，因为操作系统的内存分配是惰性的。对于已申请的内存虽然会分配地址空间，但并不会直接占用物理内存，真正使用的时候才会映射到实际的物理内存。
新生代大小：-XX:NewSize=2G、-XX:MaxNewSize=2G
设置Eden/Survivor比例：-XX:SurvivorRatio=8，注意这个表示的是两个Survivor:Eden=2:8
老年代大小：
老年代无法直接设置，只能通过堆大小+比例进行调整

设置新老一代大小之间的比率：-XX:NewRatio=2，注意这个表示的是New Size:Old Size=1:2
永久代/元空间大小：-XX:MetaspaceSize=size、-XX:MaxMetaspaceSize=size

**收集器类**
JDK8默认：-XX:+UseConcMarkSweepGC -XX:+UseParNewGC
使用G1：-XX:+UseG1GC（G1年轻代和老年代都可以回收哦！但是推荐大内存6G以上才使用G1）

**JDK8常用JVM参数组合**
JAVA_MEM_OPTS=" -server -Xmx2g -Xms2g -Xmn256m -XX:MetaspaceSize=256m **-Xss**1024m -XX:+DisableExplicitGC（不要用这个） -XX:+UseConcMarkSweepGC -XX:+CMSParallelRemarkEnabled -XX:+UseCMSCompactAtFullCollection -XX:LargePageSizeInBytes=128m -XX:+UseFastAccessorMethods -XX:+UseCMSInitiatingOccupancyOnly -XX:CMSInitiatingOccupancyFraction=70 "
JAVA_DEBUG_OPTS=" -XX:+PrintGCDetails -XX:+PrintGCDateStamps -Xloggc:/home/GCEASY/gc-%t.log -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/crashes/my-heap-dump.hprof -XX:OnOutOfMemoryError=/scripts/restart-myapp.sh "

