---
title: set注入-简单类型：value
updated: 2021-03-14T15:51:31
created: 2021-03-06T16:25:53
---

通过set方法注入，在\<bean\>标签里配置\<property\>标签，一个属性对应一个标签。

\<bean id="myStudent" class="part02.Student"\>
\<property name="age" value="15"\>\</property\>
\<property name="name" value="zhang"\>\</property\>
\</bean\>

Student std = (Student) ac.getBean("myStudent");
System.out.println(std);

注意：
set注入只去找property name指定的set方法，对应的属性是否存在没有任何关系。

相当于只是使用字符串拼接的方式，set+name就对应到了赋值的方法，而具体要干什么仅由方法内的代码决定。

因此，属性可以无，但是set必须有。

相反，只要属性有，set必须有。
