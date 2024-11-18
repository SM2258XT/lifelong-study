---
title: Mybatis步骤、# 和 $
updated: 2022-10-22T08:54:20
created: 2022-05-17T22:29:44
---

**整体步骤**
1.  创建SqlSessionFactory
2.  获取SqlSession对象（通过SqlSessionFactory）
3.  获取Mapper代理对象（通过SqlSession）
4.  执行数据库操作（通过Mapper）
5.  根据执行成功或失败，提交或回滚SqlSession事务
6.  关闭会话

**\$ 和 \#**
\$ 在大多数语法里用的最多，只是字符串的简单替换，比如JSTL、@Value（" \$ { } "）等
但是\$有 SQL 注入风险
而 \# 是Mybatis独有的，对应预编译Sql语句（PrepareStatement）
