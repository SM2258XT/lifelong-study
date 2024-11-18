---
title: 自定义配置文件@Value
updated: 2021-08-31T10:07:26
created: 2021-07-08T10:47:23
---

在application配置文件中自定义的配置，如何在java代码中使用？
1.  单属性配置映射为值。
配置：school.name=xhu

使用：@Value（"\${school.name}"） private String name; //使用@Value属性注入注解。

如果属性名和配置文件中的子属性名一样，可以不写@Value
1.  多属性配置映射为对象。
配置：

school.name=xhu

school.location=sichuan

使用：
1.  新建类，包含name和location两个属性。
2.  对类使用注解@Component和@ConfigurationProperties(prefix = "school")，将该类交给Spring管理并指定配置。
@Component

@ConfigurationProperties(prefix = "school")

public class School{ ... }

因此映射为对象，在配置文件中必须要有前缀

解决使用@ConfigurationProperties警告问题：添加一个maven依赖

