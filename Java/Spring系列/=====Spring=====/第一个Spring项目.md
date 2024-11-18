---
title: 第一个Spring项目
updated: 2021-04-12T10:38:57
created: 2021-03-06T14:55:25
---

1.  创建名字为beans.xml的文件
2.  在beans根标签下，新增一个约束：
xsi:schemaLocation="http://www.springframework.org/schema/beans <http://www.springframework.org/schema/beans/spring-beans.xsd>"

说明：

xsd是一个约束文件路径，和mybatis的dtd文件类似，但xsd约束更强

将路径复制到浏览器，是可以打开，可读的
1.  创建对象，在beans下添加标签:
**\<bean id="变量名" class="全类名" /\>**

\<bean id="someService" class="firstproject.SomeServiceImpl"/\>

注意：

class处填的必须是普通类的class，不能是抽象类和接口。
1.  获取/创建容器
ApplicationContext ac = new ClassPathXmlApplicationContext(**"beans.xml"**); //填入配置文件名

说明：

ApplicationContext表示Spring容器，通过该容器就可以获取对象了。

在容器被new出来时，会默认创建配置文件中所有的对象。

ClassPathXmlApplicationContext：表示从类路径中加载spring配置文件
1.  获取对象
SomeService service = (SomeService) ac.getBean("someService");

getBean返回Object但实际上是实现类对象，因此可以强转为接口对象。
本例使用：
interface SomeService
SomeServiceImpl implements SomeService
