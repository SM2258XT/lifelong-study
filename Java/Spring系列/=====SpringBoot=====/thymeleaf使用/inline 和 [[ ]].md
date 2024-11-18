---
title: inline 和 [[ ]]
updated: 2021-11-18T09:02:03
created: 2021-08-23T16:56:30
---

模板引擎不仅可以渲染html，也可以对JS中的对象进行预处理。而且为了在纯静态环境下可以运行，其Thymeleaf代码可以被注释起来。
\<p\>Hello, **\[\[** \${ session.user.name } **\]\]**\</p\>

如果是在javascript中获取，则必须要加 th:inline="javascript"
\<script th:inline="javascript"\>
const user = \[\[ \${user} \]\] ;
const age = \[\[ \${user.age} \]\];
\</script\>

注意：若在thymeleaf代码里存在如：var data = \[\["2012-05-07", 6\], \["2012-04-16", 4\]\]; 这样的二维数组字面量的写法，则必须要把javascript代码块设置为th:inline="none"，否则thymeleaf引擎会把此数组的\[\[也当成了内联语句处理，从而导致后端报错An error happened during template parsing。
