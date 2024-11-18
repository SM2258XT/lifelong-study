---
title: SpringMVC中的handlerAdapter
updated: 2024-09-13T19:19:13
created: 2022-10-23T16:23:45
---

org.springframework.web.servlet.HandlerAdapter

**目的：**
为了支持DispatcherServlet对原生Servlet、普通Controller、RequestMapping等方式的统一支持，采用handlerAdapter适配器接口。
ModelAndView mv = handler.handle(processedRequest, response, mappedHandler.getHandler());

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)
