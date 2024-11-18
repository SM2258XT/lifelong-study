---
title: UseCompressedOops
updated: 2022-11-22T16:43:41
created: 2022-11-22T16:39:20
---

**什么是Oops**
OOPS是指ordinary object pointers，就是原始指针。Java Runtime可以用这个指针直接访问指针对应的内存，做相应的操作（比如发起GC时做copy and sweep）

**什么是CompressedOops**
64bit的JVM出现后，OOPS的尺寸也变成了64bit，比之前的大了一倍。这会引入性能损耗——占的内存double了，并且同尺寸的CPU Cache要少存一倍的OOPS
于是就有了UseCompressedOops这个选项。打开后，OOPS变成了32bit。但32bit的base是8，所以能引用的空间是32GB——这远大于目前经常给jvm进程内存分配的空间
一般建议不要给JVM太大的内存，因为Heap太大，GC停顿实在是太久了。所以很多开发者喜欢在大内存机器上开多个JVM进程，每个给比如最大8G以下的内存。

从JDK6_u23开始UseCompressedOops被默认打开了。因此既能享受64bit带来的好处，又避免了64bit带来的性能损耗。当然，如果你有机会使用超过32G的堆内存，记得把这个选项关了。

到了Java8，永久代被干掉了，有了“meta space”的概念，存储jvm中的元数据。在UseCompressedOops之外，额外增加了一个新选项叫做UseCompressedClassPointer。
这个选项打开后，class信息中的指针也用32bit的Compressed版本。而这些指针指向的空间被称作“Compressed Class Space”。默认大小是1G，但可以通过“CompressedClassSpaceSize”调整。

如果你的java程序引用了太多的包，有可能会造成这个空间不够用，于是会看到
java.lang.OutOfMemoryError: Compressed class space
这时，一般调大CompreseedClassSpaceSize就可以了。

一般来说，平均一个 Klass 大小可以当成 1K 来算，默认的 1G 大小可以存储 100 万的 Klass。
如果遇到了 \`java.lang.OutOfMemoryError: Compressed class space\`，就是类太多了，需要结合具体情况去选择 JVM 调优还是 bug 排查。

