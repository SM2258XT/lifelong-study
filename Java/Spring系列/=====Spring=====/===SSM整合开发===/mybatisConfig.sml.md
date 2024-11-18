---
title: mybatisConfig.sml
updated: 2021-07-07T10:32:02
created: 2021-07-07T09:25:29
---

mybatisConfig.sml
2021年7月7日
9:25

jdbc.url=jdbc:mysql://localhost:3306/springdb
jdbc.userName=root
jdbc.userPwd=zhang
jdbc.maxActive=20
以上的jdbc配置文件是在applicationContext中使用的，用于给数据源赋值，因为使用的外部druid数据源而不是mybats自带的数据源。

\<configuration\>
\<!--Settings:控制全局行为--\>
\<settings\>
\<setting name="logImpl" value="STDOUT_LOGGING"/\>
\</settings\>
\<mappers\>
\<package name="com.zhang.dao"/\>
\</mappers\>
\</configuration\>

