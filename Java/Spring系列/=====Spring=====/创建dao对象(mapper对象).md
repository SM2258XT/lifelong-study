---
title: 创建dao对象(mapper对象)
updated: 2021-04-23T16:41:49
created: 2021-04-17T10:43:00
---

相当于使用sqlSession的getMapper(studentDao.class)
MapperScannerConfigurer:在内部调用getMapper()生成每个dao接口的代理对象。

\<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer"\>
\<!--指定SqlSessionFactory对象对的id--\>
\<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/\>
\<!--指定包名，写dao接口所在的包
MapperScannerConfigurer会扫描这个包中的所有接口，把每个接口都执行一次getMapper()方法，得到每个接口的dao对象。
创建好的dao对象放入到spring的容器中，自动命名为接口名(首字母小写)，因此getBean时使用的是首字母小写的接口名--\>
\<property name="basePackage" value="part06_mybatis.dao"/\>
\</bean\>

