---
title: FactoryBean和生命周期
updated: 2022-10-30T09:16:14
created: 2022-05-03T12:47:53
---

**FactoryBean和BeanFactory**
Spring有2种Bean：普通Bean、工厂Bean（**FactoryBean**）
1.  普通Bean
定义类型即返回类型
1.  FactoryBean
返回类型不一定是定义类型！（动态代理）

创建类，实现FactoryBean接口

**BeanFactory是工厂模式的一个实现**
提供了控制反转功能，用来把应用的配置和依赖从真正的应用代码中分离。
最常用的就是XmlBeanFactory，它根据XML文件中的定义加载beans。
该容器从XML 文件读取配置元数据并用它去创建一个完全配置的系统或应用。
