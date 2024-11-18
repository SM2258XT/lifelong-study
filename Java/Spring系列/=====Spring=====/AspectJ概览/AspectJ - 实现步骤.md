---
title: AspectJ - 实现步骤
updated: 2022-07-07T17:33:54
created: 2021-04-04T13:57:38
---

1.  创建普通类，在类名上使用注解@Aspect
2.  创建方法，必须符合以下要求：
public void XXX（）

如果有参数，则只能从几个类型中选择。
1.  添加注解，写切入点表达式
@Before("execution(public void doSome(String,Integer))")

public void beforeGiveMoney() {

System.out.println("切面功能：正在准备打钱！");

}
1.  创建spring-xml文件，由spring容器统一创建管理对象
\<bean id="someServiceImpl" class="part05_Aspectj.ba01.SomeServiceImpl"/\>\<!--创建业务类对象--\>

\<bean id="myAspect" class="part05_Aspectj.ba01.MyAspect"/\>\<!--创建切面类对象，不需要获取--\>

\<aop:aspectj-autoproxy/\>\<!--声明自动代理生成器--\>
1.  正常获取bean并使用即可
ApplicationContext context = new ClassPathXmlApplicationContext("part05/applicationContext.xml");

SomeService service = (SomeService) context.getBean("someServiceImpl");

service.doSome("张三", 5700);

<https://www.bilibili.com/video/BV1vs411s7BY>
magnet:?xt=urn:btih:5AB41AE48956C710787AC7DF203B784BBE7F59C3

