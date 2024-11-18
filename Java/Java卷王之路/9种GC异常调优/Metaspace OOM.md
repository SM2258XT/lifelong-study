---
title: Metaspace OOM
updated: 2022-11-20T20:00:37
created: 2022-11-20T19:51:49
---

-XX:MetaSpaceSize 和 -XX:MaxMetaSpaceSize 两个值设置为固定的，但是这样也会导致在空间不够的时候无法扩容，然后频繁地触发 GC，最终 OOM。
所以关键原因就是： ClassLoader 不停地在内存中**加载了新的 Class** ，一般这种问题都发生在动态类加载等情况上。

**策略**
可以dump快照之后，通过JPofiler或者MAT，观察Class的直方图即可，或者通过命令定位，jcmd打印几次直方图，看具体时间哪个包下的Class增加较多。
如果无法从整体的角度定位，可以添加 -XX:+TraceClassLoading 和 -XX:+TraceClassUnLoading 参数观察详细的类加载和卸载信息。

**总结**
经常会出问题的几个点有 Orika 的 classMap、JSON 的 ASMSerializer、Groovy 动态加载类等，基本都集中在反射、Javasisit 字节码增强、CGLIB 动态代理、OSGi 自定义类加载器等的技术点上。另外就是及时给 MetaSpace 区的使用率加一个监控，如果指标有波动提前发现并解决问题。
