---
title: id、namespace
updated: 2022-11-20T13:48:20
created: 2022-10-22T08:57:11
---

id、namespace
2022年10月22日
8:57

**不同的 XML 映射文件，id 是否可以重复？**
不同的 XML Mapper 文件，如果配置了 "namespace" ，那么 id 可以重复；反之则不能。毕竟"namespace" 不是必须的，只是最佳实践而已。
原理：
namespace + id 是作为 Map\<String, MappedStatement\> 的 key 使用的。如果没有 "namespace"，就剩下 id ，那么 id 重复会导致数据互相覆盖。

这个MappedStatement是指唯一接口的唯一方法。

**传递多个参数的几种方式**
1.  使用Map，不推荐
2.  @Param（"name1"）
3.  不用注解，但是根据参数在方法中的顺序，逐个使用 \#{param1}、#{param2}、#{param3}等

