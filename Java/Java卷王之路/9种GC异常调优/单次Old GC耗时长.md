---
title: 单次Old GC耗时长
updated: 2022-11-21T07:19:54
created: 2022-11-21T07:00:46
---

单次Old GC耗时长
2022年11月21日
7:00

CMS单次STW最大超过1000ms很少发生，如果频繁超过，某些场景下会引发雪崩效应，非常危险。
CMS是两阶段标记：initial mark 和 final remark

**初始标记initial mark**
开始 -\> 判断是否到达safe point -\>VM Thread进行GC Root Scan -\> 扫描YGen -\> ……
从GC Root触发标记Old，处理完成后借助BitMap处理下Young区对Old区的引用即可，整个过程还是比较快的。

**二次标记final remark（大部分问题出在这）**
开始阶段与initial mark相同，后续多了Card Table遍历、三种非强引用实例的清理、清理StringTable、CodeCache、SymbolTable等

**解决策略**
一般来说最容易出问题的地方就是 Reference 中的 FinalReference 和元数据信息处理中的 scrub symbol table 两个阶段，想要找到具体问题代码就需要内存分析工具 MAT 或 JProfiler 了，注意要 dump 即将开始 CMS GC 的堆。在用 MAT 等工具前也可以先用命令行看下对象 Histogram，有可能直接就能定位问题。

如果是FinalReference引起：找到内存来源后通过优化代码的方式来解决，如果短时间无法定位可以增加 -XX:+ParallelRefProcEnabled 对 Reference 进行并行处理。
如果是symbol table引起：观察 MetaSpace 区的历史使用峰值，以及每次 GC 前后的回收情况。一般如果没有使用动态类加载或者 DSL 处理等，MetaSpace 的使用率就不会有什么变化。这种情况可以关闭 -XX:-CMSClassUnloadingEnabled 来避免 MetaSpace 的处理，JDK8 会默认开启 CMSClassUnloadingEnabled，这会使得 CMS 在 CMS-Remark 阶段尝试进行类的卸载。
