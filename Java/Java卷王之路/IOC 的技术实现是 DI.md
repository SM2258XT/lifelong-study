---
title: IOC 的技术实现是 DI
updated: 2022-11-07T08:59:54
created: 2022-05-03T11:32:26
---

IOC 的技术实现是 DI
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
2022年5月3日
11:32

核心目的：为了降低程序耦合度
创建对象流程：
1.  根据XML进行解析
\<bean id="user" class="com.atguigu.spring5.User"\>\</bean\>
1.  创建IOC容器（对象工程类），使用反射创建对象
spring提供了2种IOC容器（2个接口）：
1.  BeanFactory：一般只是Spring内部使用。懒汉式创建对象（什么时候用什么时候才创建）
2.  ApplicationContext：继承于BeanFactory接口，饿汉式创建对象（一开始就立即创建）
1.  Bean管理
什么是Bean管理？
1.  创建对象：XML配置文件（可以用P名称空间注入来简化XML）、注解方式
2.  注入属性（**DI**）
按方式划分，可以分为
1.  **set注入**，会去找无参构造方法，如果没有会报错！：\<bean\> + \<property\>
2.  **构造方法注入**：\<bean\> + \<constructor-arg\>
1.  **接口方法注入**
如果使用注解注入，可以分为
1.  @Value：针对普通值进行注入
2.  @Autowired：byType
3.  @Qualifier：byName
4.  @Resource：byName，失败再byType
