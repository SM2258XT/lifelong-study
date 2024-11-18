---
title: 使用spring-ioc集成mybatis
updated: 2022-10-27T10:20:31
created: 2021-04-05T09:50:05
---

使用spring-ioc集成mybatis
2021年4月5日
9:50

1.  导入依赖
2.  创建domain类
Student.java
1.  创建dao类和mapper文件
StudentDao.java //interface

StudentDao.xml //mybatis-mapper文件，就在dao包下
1.  创建service类
StudentService.java //interface

StudentServiceImpl.java
1.  在resources目录下配置applicationContext.xml文件
    1.  使用bean标签，创建连接池，并使用set注入，为数据库相关信息赋值。
    2.  使用bean标签，创建SqlSessionFactory实例
2.  在resources目录下配置mybatis.xml核心文件
注意：因为要使用Druid连接池，因此原版mybatis连接池相关的\<enviroments\>标签直接全部干掉

