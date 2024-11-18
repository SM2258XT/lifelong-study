---
title: 自动注入byName
updated: 2021-03-14T16:05:06
created: 2021-03-07T19:09:24
---

自动注入只对引用类型有效！
1.  在bean标签中设置属性autowire="byName"
2.  需要自动注入的引用类型，其id必须和类中的属性名一样

\<!--自动注入--\>

\<bean id="myFather" class="part02.Father" autowire="byName"\>

\<property name="name" value="爹爹"\>\</property\>

\</bean\>

\<bean id="car" class="part02.Car"\>

\<property name="price" value="50000"\>\</property\>

\</bean\>

未解决问题：

貌似Date类自动注入有点问题？？

\<!--自动注入--\>

\<bean id="autoStd" class="part02.ComplexStudent" autowire="byName"\>

\<property name="age" value="66"\>\</property\>

\<property name="name" value="帅哥"\>\</property\>

\</bean\>

\<bean id="birthday" class="java.util.Date"\>\</bean\>

