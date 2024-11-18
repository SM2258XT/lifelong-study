---
title: set注入-引用类型：ref
updated: 2021-03-14T15:51:27
created: 2021-03-06T17:02:33
---

其实就是将新建的bean的id作为引用。
注意：
1.  使用的属性为**ref**，而不是value
2.  在任意位置写标签都可以，不用关心是否需要先声明的问题。
\<!--set注入引用类型--\>
\<bean id="myComplexStudent" class="part02.ComplexStudent"\>
\<property name="name" value="张三"\>\</property\>
\<property name="age" value="45"\>\</property\>

\<!--直接给引用类型的property传入已创建好bean的name--\>
\<property name="birthday" **ref**="myDate"\>\</property\>
\</bean\>
\<!--创建myComplexStudent所需要的引用类对象myDate--\>
\<bean id="myDate" class="java.util.Date"\>\</bean\>

