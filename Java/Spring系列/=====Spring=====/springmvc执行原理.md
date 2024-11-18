---
title: springmvc执行原理
updated: 2021-04-18T16:11:04
created: 2021-04-18T15:04:20
---

1.  tomcat启动，立即创建DispatcherServlet（因为web.xml：load-on-start优先级为1）
2.  DispatcherServlet因为仍然是一个Servlet，因此首先执行init()方法：
    1.  创建容器，读取springmvc配置文件，创建Controller对象。
相当于WebApplicationContext ctx = new ClassPathXmlApplicationContext("springmvc");
1.  把自身放入全局作用域ServletContext中。
this.getServletContext().setAttribute("springmvc",ctx);
1.  用户发起some . do请求
2.  tomcat首先获取到some.do的请求，接着tomcat根据web.xml中的url-pattern得知，\*.do应该交给DispatcherServlet处理。
3.  交给DispatcherServlet处理，和普通servlet一样，调用DispatcherServlet的方法service()：
    1.  DispatcherServlet拿到some.do的请求后，执行中央控制器的doDispatch()方法，它能根据springmvc.xml知道，some.do请求应该交给Controller01类的dosome()方法。
    2.  执行mv = ha.handle(processedRequest,response,mappedHandler.getHandler()); //调用Controller01的doSome()
4.  springmvc执行dosome()，把该方法返回的ModelAndView进行处理，并转发到show.jsp
