---
title: <mvc:>静态资源手动处理
updated: 2021-07-10T11:03:54
created: 2021-04-23T09:04:48
---

\<mvc:\>静态资源手动处理
2021年4月23日
9:04

如果仍然使用了斜杠“/”，如何解决静态资源无法处理的问题？
1.  配置\<mvc:default-servlet-handler\>，依赖Tomcat进行处理
    1.  加入注解驱动：\<mvc:annotation-driven\> 如果不加的话，会和@RequestMapping冲突报错
    2.  创建静态资源处理器：\<mvc:default-servlet-handler\>
这个处理器会把静态资源的请求，最终还是转发给tomcat-defaultServlet进行处理，依赖于Tomcat进行处理。

1.  配置\<mvc:resources\>，让框架完全自己处理（推荐）
    1.  加入注解驱动：\<mvc:annotation-driven\>
    2.  创建静态资源处理器：\<mvc:resources mapping="xx" location="xx"\>
例：

\<mvc:resources mapping="/images/\*\*" location="/images/"\> \*\*表示任何字符

常见方式：

一般是在webapp目录下，新建一个static目录，然后将该目录进行配置：

\<mvc:resources mapping="/static/\*\*" location="/static/"\>

访问资源时，就应该写src="/static/abc.jpg"
