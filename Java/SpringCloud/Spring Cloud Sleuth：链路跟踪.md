---
title: Spring Cloud Sleuth：链路跟踪
updated: 2022-02-01T19:40:58
created: 2022-02-01T19:38:06
---

1.  为什么会出现这个技术？要解决哪些问题？
在微服务框架中，一个由客户端发起的请求在后端系统中会经过多个不同的的服务节点调用来协同产生最后的请求结果，每一个前段请求都会形成一条复杂的分布式服务调用链路，链路中的任何一环出现高延时或错误都会引起整个请求最后的失败。
1.  Slueth一般和zipkin搭配使用
Slueth负责收集整理数据，zipkin负责可视化展现数据
