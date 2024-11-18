---
title: 声明数据源DataSource
updated: 2021-04-12T09:07:37
created: 2021-04-12T09:07:12
---

声明数据源DataSource
2021年4月12日
9:07

\<!--声明数据源DataSource，作用是连接数据库--\>
\<bean id="myDataSource"
class="com.alibaba.druid.pool.DruidDataSource"
init-method="init"
destroy-method="close"\>

\<property name="url" value="jdbc:mysql://localhost:3306/springdb"/\>
\<property name="username" value="root"/\>
\<property name="password" value="zhang"/\>
\<property name="maxActive" value="20"/\>
\</bean\>
