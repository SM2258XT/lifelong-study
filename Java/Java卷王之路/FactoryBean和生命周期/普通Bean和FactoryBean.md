---
title: 普通Bean和FactoryBean
updated: 2022-10-30T16:19:03
created: 2022-07-04T14:36:32
---

Spring 中有两种类型的Bean：一种是普通Bean，另一种是工厂Bean 即 FactoryBean。
FactoryBean跟普通Bean不同，其返回的对象不是指定类的一个实例，而是该FactoryBean的**getObject方法所返回的对象**。
创建出来的对象是否属于单例由isSingleton中的返回决定。

一般情况下，Spring通过反射机制利用\<bean\>的class属性指定实现类实例化Bean，在某些情况下，实例化Bean过程比较复杂，如果按照传统的方式，则需要在\<bean\>中提供大量的配置信息。配置方式的灵活性是受限的，这时采用编码的方式可能会得到一个简单的方案。Spring为此提供了一个org.springframework.bean.factory.FactoryBean的工厂类接口，用户可以通过实现该接口定制实例化Bean的逻辑。FactoryBean接口对于Spring框架来说占用重要的地位，Spring自身就提供了70多个FactoryBean的实现。它们隐藏了实例化一些复杂Bean的细节，给上层应用带来了便利。从Spring3.0开始，FactoryBean开始支持泛型，即接口声明改为FactoryBean的形式

以Bean结尾，表示它是一个Bean，不同于普通Bean的是：它是实现了FactoryBean接口的Bean，根据该Bean的ID从BeanFactory中获取的实际上是FactoryBean的getObject()返回的对象，而不是FactoryBean本身，如果要获取FactoryBean对象，请在id前面加一个&符号来获取。

public class UserServiceFactoryBean implements FactoryBean\<UserService\> {
/\*\*
\* 使用FactoryBean来实现以下xml配置的效果：
\* \<bean id="roleDAO" class="com.hhh.dao.impl.RoleDAOImpl"\>\</bean\>
\* \<bean id="userDAO" class="com.hhh.dao.impl.UserDAOImpl"\>\</bean\>
\* \<bean id="userService" class="com.hhh.service.impl.UserServiceImpl"\>
\* \<property name="userDAO" ref="userDAO"\>\</property\>
\* \<property name="roleDAO" ref="roleDAO"\>\</property\>
\* \</bean\>
\* @return
\* @throws Exception
通过FactoryBean返回指定类型的对象
\*/
@Override
public UserService **getObject()** throws Exception {
//编写复杂的组装逻辑
UserServiceImpl userService = new UserServiceImpl();
userService.setUserDAO(new UserDAOImpl());
return userService;
}
//当前对象的类型
@Override
public Class\<?\> **getObjectType**() {
return null;
}
//返回的对象是否为单例
@Override
public boolean **isSingleton**() {
return true;
}
}
