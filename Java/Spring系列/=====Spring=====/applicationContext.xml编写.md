---
title: applicationContext.xml编写
updated: 2021-04-23T16:17:20
created: 2021-04-17T10:46:52
---

1.  [创建Druid连接池](onenote:#声明数据源DataSource&section-id={35CFED24-F00C-4E25-AAC8-CD4FED7F33AD}&page-id={4956DF9D-1D4A-4DD5-A5AB-05068E9A703B}&end&base-path=https://d.docs.live.net/36a2ce0fd7a6557d/文档/Java/Spring.one)
2.  [创建sqlSessionFactory](onenote:#创建SqlSessionFactory&section-id={35CFED24-F00C-4E25-AAC8-CD4FED7F33AD}&page-id={0EA0BAFB-CEBE-4304-B60C-E69B3BA0D6D1}&end&base-path=https://d.docs.live.net/36a2ce0fd7a6557d/文档/Java/Spring.one)
3.  [创建mapper对象(mybatis)](onenote:#创建dao对象&section-id={35CFED24-F00C-4E25-AAC8-CD4FED7F33AD}&page-id={C6E5B58C-AC74-4DA5-B6DA-18BF0B222434}&end&base-path=https://d.docs.live.net/36a2ce0fd7a6557d/文档/Java/Spring.one)

完整代码模板：

\<!--以下代码几乎都是固定不变的--\>

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

\<!--创建SqlSessionFactory--\>

\<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"\>

\<property name="dataSource" ref="myDataSource"/\>

\<property name="configLocation" value="classpath:/part06_mybatis/MyBatisConfig.xml"/\>

\</bean\>

\<!--创建dao类对象

相当于使用sqlSession的getMapper(studentDao.class)

MapperScannerConfigurer:在内部调用getMapper()生成每个dao接口的代理对象。

--\>

\<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer"\>

\<!--指定SqlSessionFactory对象对的id--\>

\<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/\>

\<!--指定包名，写dao接口所在的包

MapperScannerConfigurer会扫描这个包中的所有接口，把每个接口都执行一次getMapper()方法，得到每个接口的dao对象。

创建好的dao对象放入到spring的容器中的。--\>

\<property name="basePackage" value="part06_mybatis.dao"/\>

\</bean\>

