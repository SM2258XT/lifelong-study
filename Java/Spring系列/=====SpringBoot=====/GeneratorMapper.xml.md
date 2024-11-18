---
title: GeneratorMapper.xml
updated: 2021-07-09T16:54:32
created: 2021-07-08T15:16:07
---

\<?xml version="1.0" encoding="UTF-8"?\>
\<!DOCTYPE generatorConfiguration
PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
"<http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd>"\>
\<generatorConfiguration\>
\<classPathEntry location="C:\Code\repository\mysql\mysql-connector-java\5.1.35\mysql-connector-java-5.1.35.jar"/\>
\<context id="tables" targetRuntime="MyBatis3"\>
\<commentGenerator\>
\<property name="suppressAllComments"
value="true"/\>
\</commentGenerator\>
\<jdbcConnection driverClass="com.mysql.jdbc.Driver"
connectionURL="jdbc:mysql://localhost:3306/springdb"
userId="root"
password="zhang"\>
\</jdbcConnection\>

\<javaModelGenerator targetPackage="com.example.demo.model"
targetProject="src/main/java"\>
\<property name="enableSubPackages" value="false"/\>
\<property name="trimStrings" value="false"/\>
\</javaModelGenerator\>
\<sqlMapGenerator targetPackage="com.example.demo.mapper"
targetProject="src/main/java"\>
\<property name="enableSubPackages" value="false"/\>
\</sqlMapGenerator\>
\<javaClientGenerator type="XMLMAPPER"
targetPackage="com.example.demo.mapper"
targetProject="src/main/java"\>
\<property name="enableSubPackages" value="false"/\>
\</javaClientGenerator\>

\<table tableName="t_student"
domainObjectName="Student"
enableCountByExample="false"
enableUpdateByExample="false"
enableDeleteByExample="false"
enableSelectByExample="false"
selectByExampleQueryId="false"\>
\</table\>
\</context\>
\</generatorConfiguration\>
