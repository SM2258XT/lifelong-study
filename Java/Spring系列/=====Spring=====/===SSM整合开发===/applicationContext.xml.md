---
title: applicationContext.xml
updated: 2021-07-07T09:31:23
created: 2021-07-07T09:24:16
---

applicationContext.xml
2021年7月7日
9:24

对应于spring容器

\<context:property-placeholder location="classpath:mainConf/jdbc.properties"/\>
\<!--声明数据源DataSource，作用是连接数据库--\>
\<bean id="myDataSource"
class="com.alibaba.druid.pool.DruidDataSource"
init-method="init"
destroy-method="close"\>
\<property name="url" value="\${jdbc.url}"/\>
\<property name="username" value="\${jdbc.userName}"/\>
\<property name="password" value="\${jdbc.userPwd}"/\>
\<property name="maxActive" value="\${jdbc.maxActive}"/\>
\</bean\>

\<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"\>
\<property name="dataSource" ref="myDataSource"/\>
\<property name="configLocation" value="classpath:/mainConf/mybatisConfig.xml"/\>
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

\<!--声明service所在位置--\>
\<context:component-scan base-package="com.zhang.service"/\>
