---
title: diapaterServlet.xml
updated: 2021-07-16T15:44:05
created: 2021-04-23T16:12:59
---

对应于springMVC容器：

\<context:component-scan base-package="com.zhang.controller"/\>
\<!--声明视图解析器，帮助开发人员设置视图文件路径--\>
\<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"\>
\<property name="prefix" value="/WEB-INF/jsp/"/\>
\<property name="suffix" value=".jsp"/\>
\</bean\>
\<!--注解驱动--\>
\<mvc:annotation-driven/\>
