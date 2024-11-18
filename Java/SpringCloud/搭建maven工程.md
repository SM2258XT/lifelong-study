---
title: 搭建maven工程
updated: 2021-10-01T18:28:38
created: 2021-10-01T16:00:39
---

\<dependencyManagement\>
\<dependencies\>
\<!-- springboot 2.2.2 --\>
\<dependency\>
\<groupId\>org.springframework.boot\</groupId\>
\<artifactId\>spring-boot-dependencies\</artifactId\>
\<version\>2.2.2.RELEASE\</version\>
\<type\>pom\</type\>
\<scope\>import\</scope\>
\</dependency\>
\<!-- spring cloud Hoxton.SR1 --\>
\<dependency\>
\<groupId\>org.springframework.cloud\</groupId\>
\<artifactId\>spring-cloud-dependencies\</artifactId\>
\<version\>Hoxton.SR1\</version\>
\<type\>pom\</type\>
\<scope\>import\</scope\>
\</dependency\>
\<!-- spring cloud alibaba 2.1.0.RELEASE --\>
\<dependency\>
\<groupId\>com.alibaba.cloud\</groupId\>
\<artifactId\>spring-cloud-alibaba-dependencies\</artifactId\>
\<version\>2.1.0.RELEASE\</version\>
\<type\>pom\</type\>
\<scope\>import\</scope\>
\</dependency\>
\<dependency\>
\<groupId\>mysql\</groupId\>
\<artifactId\>mysql-connector-java\</artifactId\>
\<version\>\${mysql.version}\</version\>
\</dependency\>
\<dependency\>
\<groupId\>com.alibaba\</groupId\>
\<artifactId\>druid\</artifactId\>
\<version\>\${druid.version}\</version\>
\</dependency\>
\<dependency\>
\<groupId\>org.mybatis.spring.boot\</groupId\>
\<artifactId\>mybatis-spring-boot-starter\</artifactId\>
\<version\>\${mybatis.spring.boot.version}\</version\>
\</dependency\>
\<dependency\>
\<groupId\>log4j\</groupId\>
\<artifactId\>log4j\</artifactId\>
\<version\>\${log4j.version}\</version\>
\</dependency\>
\<dependency\>
\<groupId\>junit\</groupId\>
\<artifactId\>junit\</artifactId\>
\<version\>\${junit.version}\</version\>
\</dependency\>
\<dependency\>
\<groupId\>org.projectlombok\</groupId\>
\<artifactId\>lombok\</artifactId\>
\<version\>\${lombok.version}\</version\>
\<optional\>true\</optional\>
\</dependency\>
\</dependencies\>
\</dependencyManagement\>
\<properties\>
\<project.build.sourceEncoding\>UTF-8\</project.build.sourceEncoding\>
\<maven.compiler.source\>1.8\</maven.compiler.source\>
\<maven.compiler.target\>1.8\</maven.compiler.target\>
\<junit.version\>4.12\</junit.version\>
\<log4j.version\>1.2.17\</log4j.version\>
\<lombok.version\>1.18.0\</lombok.version\>
\<mysql.version\>5.1.47\</mysql.version\>
\<druid.version\>1.1.16\</druid.version\>
\<mybatis.spring.boot.version\>1.3.2\</mybatis.spring.boot.version\>
\</properties\>

创建微服务模块五部曲：
1.  建module
2.  改pom
3.  写yml
4.  主启动
5.  业务类
