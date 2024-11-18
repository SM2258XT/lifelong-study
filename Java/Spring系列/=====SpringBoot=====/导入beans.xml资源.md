---
title: 导入beans.xml资源
updated: 2022-05-22T19:20:53
created: 2021-07-15T11:16:37
---

在springboot中一般使用配置类创建组件，也可以兼容spring-xml配置文件，但是需要告诉springboot在哪儿去找这个配置文件？

对任意配置类使用注解@ImportResource（路径）

例：
1.  在resources目录下创建一个beans.xml配置文件
2.  对任意配置类使用注解：@ImportResource（"classpath:beans.xml"）
这样就可以导入beans.xml配置文件
