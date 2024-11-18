---
title: deafultServlet静态资源默认处理
updated: 2021-04-23T09:06:23
created: 2021-04-23T08:36:52
---

deafultServlet静态资源默认处理
2021年4月23日
8:36
我们手动配置的dispatcherServlet如果指定去处理\*.do的资源，那么其他没有被指定的静态资源（js,图片,css）是如何被处理的？
在conf/web.xml中，Tomcat配置了一个默认的资源处理器：deafult
该dedfaultServlet已经实现了处理静态资源的能力

\<servlet\>

\<servlet-name\>default\</servlet-name\>

。。。。。

\<load-on-startup\>1\</load-on-startup\>

\</servlet\>

\<servlet-mapping\>

\<servlet-name\>default\</servlet-name\>

\<url-pattern\>**/**\</url-pattern\>

\</servlet-mapping\>
因此，如果自己的servlet映射的是斜杠“**/**”，则会代替tomcat-defaultServlet，就没有默认的静态资源处理方式，就一定会出错。
