---
title: 创建SqlSessionFactory
updated: 2021-04-14T16:41:41
created: 2021-04-14T16:39:55
---

\<!--创建SqlSessionFactoryBean--\>
\<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean"\>
\<property name="dataSource" ref="myDataSource"/\>
\<property name="configLocation" value="classpath:part06_mybatis/MyBatisConfig.xml"/\>
\</bean\>

注意：因为mybatis核心文件是在resourse目录下的，所有非类路径都要使用classpath:来指定
