---
title: AspectJ - jdk和cglib动态代理
updated: 2022-10-27T10:20:25
created: 2021-04-05T09:40:31
---

AspectJ - jdk和cglib动态代理
2021年4月5日
9:40

结论：
目标类实现了接口的是jdk动态代理；

仅仅是普通类和类继承的是cglib动态代理。

使用System.out.println(abc.getClass().getName());可以查看代理类名称：

JDK动态代理：com.sun.proxy.\$Proxy11

cglib动态代理：proxy: com. bjpowernode.ba07. SomeServiceImpl\$\$EnhancerBySpringCGLIB\$\$575c8b90

有接口，默认使用JDK，如果仍然想用cglib代理？在spring-config.xml中，配置以下标签：
\<aop: aspectj-autoproxy proxy-target-class="true"/\>
