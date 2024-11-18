---
title: 集成MyBatis（1）逆向工程
updated: 2022-06-17T16:53:57
created: 2021-07-08T14:49:03
---

可以暂时不管这里的逆向工程，后面使用mp-generator或者人人开源、若依逆向工程，效果更好
1.  添加mysql依赖和mybatis依赖
\<dependency\>

\<groupId\>mysql\</groupId\>

\<artifactId\>mysql-connector-java\</artifactId\>

\<version\>5.1.9\</version\>

\</dependency\>

\<dependency\>

\<groupId\>org.mybatis.spring.boot\</groupId\>

\<artifactId\>mybatis-spring-boot-starter\</artifactId\>

\<version\>2.0.0\</version\>

\</dependency\>

1.  在与src同级目录中创建[GeneratorMapper.xml](onenote:#GeneratorMapper.xml&section-id={77520B5A-130A-4289-8F52-789FCE13F901}&page-id={A4879115-46B1-486E-AF5C-39047CC87916}&end&base-path=https://d.docs.live.net/36a2ce0fd7a6557d/文档/Java/SpringBoot.one)并粘贴修改配置。
注意：每一张数据库的表，对应一个\<table\>标签
1.  在pom中添加插件
\<!--mybatis逆向工程插件--\>

\<plugin\>

\<groupId\>org.mybatis.generator\</groupId\>

\<artifactId\>mybatis-generator-maven-plugin\</artifactId\>

\<version\>1.3.6\</version\>

\<configuration\>

\<configurationFile\>GeneratorMapper.xml\</configurationFile\>

\<verbose\>true\</verbose\>

\<overwrite\>true\</overwrite\>

\</configuration\>

\</plugin\>
1.  执行
打开maven菜单的mybatis-generator，双击第一个选项即可。
