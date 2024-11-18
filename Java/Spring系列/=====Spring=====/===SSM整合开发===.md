---
title: ===SSM整合开发===
updated: 2021-04-23T15:29:52
created: 2021-04-23T09:29:13
---

===SSM整合开发===
2021年4月23日
9:29

- 调用过程：
用户发起请求 -\> springMVC接收 -\> spring中的service处理 -\> MyBatis处理数据
- 其中涉及到2个容器：
  1.  Spring容器：管理Service，Dao，Utils类对象。
将对象创建放在spring-config.xml文件中，容器的创建使用new ClassPathXML...("路径");
1.  SpringMVC容器：管理Controller控制器对象。
将对象创建放在springMVC-config.xml文件中，将容器的创建放在web.xml中。

- springMVC容器是spring容器的子容器，类似于继承关系，所以关联就在于springMVC可以访问spring的内容
所以mvc中的Controller就可以访问并使用spring中的service对象
