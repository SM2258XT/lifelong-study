---
title: 创建mybatis-config核心文件
updated: 2021-04-23T19:07:25
created: 2021-04-23T19:03:22
---

\<?xml version="1.0" encoding="UTF-8" ?\>

\<!DOCTYPE configuration

PUBLIC "-//mybatis.org//DTD Config 3.0//EN"

"<http://mybatis.org/dtd/mybatis-3-config.dtd>"\>

\<configuration\>

\<!--Settings:控制全局行为--\>

\<settings\>

\<setting name="logImpl" value="STDOUT_LOGGING"/\>

\</settings\>

\<!--设置别名--\>

\<typeAliases\>

\<package name="com.zhang.domain"/\>

\</typeAliases\>

\<mappers\>

\<package name="com.zhang.dao"/\>

\</mappers\>

\</configuration\>

注意，使用package要求：
1.  name是包名，指定包中所有mapper.xml都能一次性加载
2.  mapper文件名和dao接口名必须完全一样，包括大小写
3.  mapper文件和dao接口必须放在同一目录下
