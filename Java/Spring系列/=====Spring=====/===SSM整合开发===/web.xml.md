---
title: web.xml
updated: 2021-04-23T16:04:44
created: 2021-04-23T15:55:30
---

\<!--创建springmvc核心对象DispatcherServlet--\>
\<servlet\>
\<servlet-name\>springmvc\</servlet-name\>
\<servlet-class\>org.springframework.web.servlet.DispatcherServlet\</servlet-class\>
\<!--自定义springmvc配置文件位置--\>
\<init-param\>
\<param-name\>contextConfigLocation\</param-name\>
\<param-value\>classpath:mainConf/dispatcherServlet.xml\</param-value\>
\</init-param\>
\<!--在启动时就立即创建该对象，数值越小则优先级越高--\>
\<load-on-startup\>1\</load-on-startup\>
\</servlet\>

\<servlet-mapping\>
\<servlet-name\>springmvc\</servlet-name\>
\<url-pattern\>\*.do\</url-pattern\>
\</servlet-mapping\>
\<!--注册spring的监听器--\>
\<context-param\>
\<param-name\>contextConfigLocation\</param-name\>
\<param-value\>classpath:mainConf/applicationContext.xml\</param-value\>
\</context-param\>
\<listener\>
\<listener-class\>org.springframework.web.context.ContextLoaderListener\</listener-class\>
\</listener\>
\<!--注册字符集过滤器--\>
\<filter\>
\<filter-name\>characterEncodingFilter\</filter-name\>
\<filter-class\>org.springframework.web.filter.CharacterEncodingFilter\</filter-class\>
\<init-param\>
\<param-name\>encoding\</param-name\>
\<param-value\>utf-8\</param-value\>
\</init-param\>
\<init-param\>
\<param-name\>forceRequestEncoding\</param-name\>
\<param-value\>true\</param-value\>
\</init-param\>
\<init-param\>
\<param-name\>forceResponseEncoding\</param-name\>
\<param-value\>true\</param-value\>
\</init-param\>
\</filter\>
\<filter-mapping\>
\<filter-name\>characterEncodingFilter\</filter-name\>
\<url-pattern\>/\*\</url-pattern\>
\</filter-mapping\>
