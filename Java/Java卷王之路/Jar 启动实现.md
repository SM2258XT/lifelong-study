---
title: Jar 启动实现
updated: 2022-09-04T07:45:53
created: 2022-09-01T16:06:49
---

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
**Jar包结构**
打开一个Jar包，主要有3个文件夹：
1.  META-MF
通过 MANIFEST.MF 文件提供 jar 包的元数据，声明了 jar 的启动类。
1.  org
核心！提供**spring-boot-loader**用于支持java -jar
1.  BOOT-INF
    1.  BOOT-INF/lib
依赖的jars，可以嵌套
1.  BOOT-INF/classes
编译后文件，主要是用户程序

**不是符合java规范的标准jar包！**
spring-boot-loader 项目需要解决两个问题：
1.  如何引导执行我们创建的 Spring Boot 应用的启动类（ Application 类）。
2.  如何加载 BOOT-INF/class 目录下的类，以及 BOOT-INF/lib 目录下内嵌的 jar 包中的类。

**MANIFEST.MF**
1.  Main-Class: org.springframework.boot.loader.JarLauncher
指定Java所规定需要的jar包启动类，这里指定的是JarLauncher
1.  Start-Class: cn.iocoder.springboot.lab39.skywalkingdemo.Application
指定SpringBoot自定义的主启动类，即用户Application
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)
