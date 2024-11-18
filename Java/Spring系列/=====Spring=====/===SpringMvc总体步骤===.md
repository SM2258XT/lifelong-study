---
title: ===SpringMvc总体步骤===
updated: 2022-07-07T15:48:19
created: 2021-03-20T17:14:42
---

1.  新建maven-web工程
2.  [加入依赖](onenote:Maven.one#spring-webmvc&section-id={69E24AE7-C8E4-4770-A338-BCD237E82D07}&page-id={91B461EA-996F-4A60-91C3-9423E9083A59}&end&base-path=https://d.docs.live.net/36a2ce0fd7a6557d/文档/Java)：
spring-webmvc
1.  [在web.xml中注册springmvc框架核心对象DispatcherServlet](onenote:#注册DispatcherServlet&section-id={40375F78-D8D5-463A-BBD7-8CE6EA3C7C1A}&page-id={BC219D75-B7F8-4F07-AD40-1266A356A064}&end&base-path=https://d.docs.live.net/36a2ce0fd7a6557d/文档/Java/SpringMVC.one)
DispatcherServlet：中央调度器。是一个Servlet，继承于HttpServlet，也叫前端控制器，负责接收用户请求，调用其他控制器对象，并把调用结果返回给用户。
1.  [创建Controller控制器类](onenote:#创建Controller控制器&section-id={40375F78-D8D5-463A-BBD7-8CE6EA3C7C1A}&page-id={90DACF26-D9ED-41B3-B0B1-4A2258AB280B}&end&base-path=https://d.docs.live.net/36a2ce0fd7a6557d/文档/Java/SpringMVC.one)
    1.  在类上加入@Controller注解，创建对象并放入springmvc程序中
    2.  在类方法上加入@RequestMapping注解
2.  [创建请求和结果展示jsp](onenote:#创建req.jsp，res.jsp&section-id={40375F78-D8D5-463A-BBD7-8CE6EA3C7C1A}&page-id={4A91F5ED-F745-4A3F-9BE5-92CC6722653A}&end&base-path=https://d.docs.live.net/36a2ce0fd7a6557d/文档/Java/SpringMVC.one)：发起请求request.jsp，创建结果展示response.jsp
3.  [创建springmvc配置文件](onenote:#创建springmvc配置文件&section-id={40375F78-D8D5-463A-BBD7-8CE6EA3C7C1A}&page-id={66922FC7-92AB-4A69-B6D1-D46C2060F0A6}&end&base-path=https://d.docs.live.net/36a2ce0fd7a6557d/文档/Java/SpringMVC.one)（和spring一样）
    1.  声明组件扫描器，指定@Controller所在包名。
    2.  声明视图解析器，帮助处理视图。
