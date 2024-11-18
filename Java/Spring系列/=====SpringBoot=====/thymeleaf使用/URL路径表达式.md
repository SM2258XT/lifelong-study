---
title: URL路径表达式
updated: 2021-08-23T16:38:22
created: 2021-08-23T16:04:50
---

语法：**th:href**=" **@** { url } "

\<a href="test02"\>传统写法\</a\>
\<a th:href="@{/test02}"\>URL路径表达式\</a\>
\<a th:href="@{/test02**?**username=zhangsan}"\>URL路径表达式，并附带写死的参数\</a\>
\<a th:href="@{ **'** /test02?username= **'** + \$ { username } }"\>从后台获取参数，将其又作为新的url参数，注意单引号！\</a\>
\<a th:href="@{ /test02 **(** username=\${username} , age=\${age} , money=\${money} **)** }"\>简化版，又是经典白学。。。\</a\>

同理：
th:src
th:action

