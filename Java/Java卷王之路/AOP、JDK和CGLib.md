---
title: AOP、JDK和CGLib
updated: 2022-10-27T12:20:59
created: 2022-05-17T22:26:19
---

**cglib和jdk动态代理性能对比**
Cglib需要生成字节码，生成字节码的时候比较缓慢，但是生成后的效率很高，jdk的需要invoke，效率一般，但是不用生成字节码

**@Scope使用Prototype**
@Scope(value="prototype",proxyMode=TARGET_CLASS)配置为原型模式，这个代理模式可以改变
Spring1.0代理使用的是JDK，所以必须先定义接口，再实现接口变成Service
2.0开始，使用CGLib，这时候可以发现即使不定义接口，也可以使用，原因就是CGLib时使用类继承实现的
