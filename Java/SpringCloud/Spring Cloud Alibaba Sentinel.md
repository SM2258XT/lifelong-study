---
title: Spring Cloud Alibaba Sentinel
updated: 2022-02-02T16:35:52
created: 2021-10-12T08:15:49
---

Spring Cloud Alibaba Sentinel
2021年10月12日
8:15

Sentinel 是什么？
随着微服务的流行，服务和服务之间的稳定性变得越来越重要。Sentinel 以流量为切入点，从流量控制、熔断降级、系统负载保护等多个维度保护服务的稳定性。
Sentinel 分为两个部分：
核心库（Java 客户端）：不依赖任何框架/库，能够运行于所有 Java 运行时环境，同时对 Dubbo / Spring Cloud 等框架也有较好的支持。

控制台（Dashboard）：基于 Spring Boot 开发，打包后可以直接运行，不需要额外的 Tomcat 等应用容器。

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
