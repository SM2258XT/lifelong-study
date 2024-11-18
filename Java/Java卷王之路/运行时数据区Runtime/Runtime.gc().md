---
title: Runtime.gc()
updated: 2022-09-15T15:28:30
created: 2022-05-08T22:00:02
---

**作用：**显式触发Full GC，但不一定立即生效
**面试：**System.gc()和Runtime.gc()区别？
System.gc()就是个套壳：

public static void gc() {

Runtime.getRuntime().gc();

}

而Runtime.gc()才是真正的native方法
