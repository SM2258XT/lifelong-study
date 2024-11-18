---
title: SpringMVC流程
updated: 2022-10-29T10:47:54
created: 2022-10-21T12:53:56
---

**整体流程**
1.  **DispatcherServlet**
2.  **映射处理器**
DispatcherServlet \# doDispatch（）里面，调用getHandler（），获取URI对应的处理器和拦截器

HandlerExecutionChain handler = mapping.getHandler(request);
1.  **处理器适配**
**注意：**当下主流前后端分离，所以用不到后面的View和Resolver了。当前handler执行完后，**如果有@ResponseBody，则直接将结果返回浏览器**
1.  DispatcherServlet 根据获得的handler，选一个合适的HandlerAdapter
2.  执行拦截器preHandler（）方法
3.  提取请求数据，处理后填充至handler入参，执行handler（controller）
    1.  HttpMessageConverter ：会将请求消息（如 JSON、XML 等数据）转换成一个对象。例如：FastJsonHttpMessageConverter
    2.  数据转换：对请求消息进行数据转换。如 String 转换成 Integer、Double 等。
    3.  数据格式化：对请求消息进行数据格式化。如将字符串转换成格式化数字或格式化日期等。
    4.  数据验证： 验证数据的有效性（长度、格式等），验证结果存储到 BindingResult 或 Error 中。
4.  向DispatcherServlet 返回一个 ModelAndView 对象。
1.  **解析视图**
根据返回的 ModelAndView ，选择一个适合的 ViewResolver（必须是已经注册到 Spring 容器中的 ViewResolver)，解析出 View 对象，然后返回给 DispatcherServlet。
1.  **渲染视图+响应请求**
ViewResolver 结合 Model 和 View，来渲染视图，并写回给用户( 浏览器 )。
