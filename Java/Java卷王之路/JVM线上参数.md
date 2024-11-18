---
title: JVM线上参数
updated: 2022-11-29T08:25:57
created: 2022-10-28T17:07:00
---

XMS：最小Heap；XMX：最大Heap

JVM调优
CMD \["java","-jar","-server","-Dfile.encoding=UTF-8",
"-Xms2048m","-Xmx2048m", // 设置堆（仅Eldn+Old）最小最大相同，避免每次GC都重新分配内存
"-XX:MetaspaceSize=256m","-XX:MaxMetaspaceSize=256m", // 不是PermGen哈！！！
"-XX:+UseCompressedOops", // 指针压缩，对象头16字节(klass pointer 8字节)reference 8字节 -\> 对象头12字节(klass pointer 4字节) reference 4字节
"-XX:+AlwaysPreTouch", // 立即分配物理内存，而不是虚拟内存
"-XX:+HeapDumpOnOutOfMemoryError", // JVM发生OOM时，自动生成DUMP文件
"-XX:-OmitStackTraceInFastThrow", // jvm检测到程序在重复抛一个异常，在执行若干次（JDK7为20707次）后会将异常吞掉，stackTrace 长度会为0。有时这不利于排错
"-Djava.net.preferIPv4Stack=true", // 只支持IPV4
"-ea","app.jar"\]

其他
-XX:ReservedCodeCacheSize=128m
-XX:InitialCodeCacheSize=128m,
-Xss512k
-XX:+UseG1GC
-XX:G1HeapRegionSize=4M
MaxDirectMemorySize

**AlwaysPreTouch：**启动初期有些操作系统（例如 Linux）并没有真正分配物理内存给 JVM ，而是在虚拟内存中分配，使用的时候才会在物理内存中分配内存页，这样也会导致 GC 时间较长。这种情况可以添加 -XX:+AlwaysPreTouch 参数，让 VM 在 commit 内存时跑个循环来强制保证申请的内存真的 commit，避免运行时触发缺页异常。在一些大内存的场景下，有时候能将前几次的 GC 时间降一个数量级，但是添加这个参数后，启动的过程可能会变慢。
