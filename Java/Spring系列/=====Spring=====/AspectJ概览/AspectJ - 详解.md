---
title: AspectJ - 详解
updated: 2022-07-07T15:49:59
created: 2021-04-04T14:29:16
---

1.  切面类需要使用\<bean\>创建对象，但无需获取。
2.  通过applicationContext获取到的实际上已经不是原有的类，而是一个jdk动态代理的类！
System.out.println(service); ---\> com.sun.proxy.\$Proxy8
1.  执行原理（xml执行时）
    1.  创建所有bean
    2.  执行到\<aop:scpectj-autoproxy/\>时，开始扫描所有bean对应类带有@AspectJ的注解，找出所有切面类。
    3.  根据execution表达式找到所有目标切面方法，生成对应的代理对象。
