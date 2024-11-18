---
title: Unsafe：魔法类
updated: 2022-11-21T19:01:18
created: 2022-09-12T09:56:37
---

Unsafe：魔法类
<https://tech.meituan.com/2019/02/14/talk-about-java-magic-class-unsafe.html>
2022年9月12日
9:56

主要提供一些用于执行低级别、不安全操作的方法，如直接访问系统内存资源、自主管理内存资源等，这些方法在提升Java运行效率、增强Java语言底层资源操作能力方面起到了很大的作用。但由于Unsafe类使Java语言拥有了类似C语言指针一样操作内存空间的能力，这无疑也增加了程序发生相关指针问题的风险。在程序中过度、不正确使用Unsafe类会使得程序出错的概率变大，使得Java这种安全的语言变得不再“安全”，因此对Unsafe的使用一定要慎重。

**获取Unsafe单例实例**
Unsafe类为一单例实现，提供静态方法getUnsafe获取Unsafe实例，当且仅当调用getUnsafe方法的类为引导类加载器所加载时才合法，否则抛出SecurityException异常。
如若想使用这个类，该如何获取其实例？有如下两个可行方案。
1.  从getUnsafe方法的使用限制条件出发，通过Java命令行命令-Xbootclasspath/a，把调用Unsafe相关方法的类A所在jar包路径追加到默认的bootstrap路径中，使得A被引导类加载器加载，从而通过Unsafe.getUnsafe方法安全的获取Unsafe实例。
java -Xbootclasspath/a: \${path} // path为调用Unsafe相关方法的类所在jar包路径
1.  通过反射获取单例对象theUnsafe。
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
