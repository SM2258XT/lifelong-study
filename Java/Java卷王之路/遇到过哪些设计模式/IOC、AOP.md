---
title: IOC、AOP
updated: 2022-11-12T08:23:28
created: 2022-11-07T08:54:42
---

IOC、AOP
2022年11月7日
8:54

**IOC相关方面**
1.  技术实现 DI：Bean生命周期、装配过程、三级缓存、循环依赖
2.  项目有关的自动装配：database-boot-starter
    1.  相关注解：@Import\*注入Properties
    2.  @Component不能扫描

**AOP相关**
1.  代理原理：JDK和CgLib
2.  Spring事务失效原因之一：方法内调用，没有经过切面
