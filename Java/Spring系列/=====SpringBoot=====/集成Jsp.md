---
title: 集成Jsp
updated: 2021-07-08T11:21:14
created: 2021-07-08T11:06:14
---

由于SpringBoot创建时只是一个普通Maven项目，因此要先创建webapp目录。
1.  在main下创建目录webapp，此时只是一个普通文件夹。
2.  进行[项目结构设置](onenote:#项目结构设置&section-id={77520B5A-130A-4289-8F52-789FCE13F901}&page-id={C9EF8884-51D1-43D5-B7E2-D175BA061CB9}&end&base-path=https://d.docs.live.net/36a2ce0fd7a6557d/文档/Java/SpringBoot.one)。
3.  添加jsp依赖：
\<dependency\>

\<groupId\>org.apache.tomcat.embed\</groupId\>

\<artifactId\>tomcat-embed-jasper\</artifactId\>

\</dependency\>
1.  在build下手动指定jsp编译位置。
\<resources\>

\<resource\>

\<directory\>src/main/webapp\</directory\>

\<targetPath\>META-INF/resources\</targetPath\>

\<includes\>

\<include\>\*.\*\</include\>

\</includes\>

\</resource\>

\</resources\>
1.  在application中配置视图解析器。
spring.mvc.view.prefix=/

spring.mvc.view.suffix=.jsp
