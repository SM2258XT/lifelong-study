---
title: '``Bean初始化流程'
updated: 2023-03-02T17:38:53
created: 2022-10-30T10:33:03
---

AbstractBeanFactory#doGetBean(final String name, @Nullable final Class\<T\> requiredType,@Nullable final Object\[\] args, boolean typeCheckOnly)

**============== Bean 加载**
1.  **获取名称并进行转换**
方法传递的name，还有可能是aliasName、FactoryBean（Name）等，所以要先转换一下
1.  **从缓存中获取Bean**
第一次创建后会将该 Bean 加载到缓存中。后面，在获取 Bean 就会直接从单例缓存中获取。

获取之后，还需要进行实例化处理，因为缓存中记录的是最原始Bean状态
1.  **原型模式依赖检查**
Spring只处理Singleton模式下的循环依赖。对于Prototype模式则直接抛异常。

循环依赖解决策略：
1.  Singleton单例模式下
我们知道Bean并不是完全创建完后才加入缓存，而是不等创建完成，就会将Bean的ObjectFactory提早加入缓存

这样一旦后续某个Bean有依赖时，直接使用ObjectFactory
1.  Prototype原型模式下
原型是没法用缓存的，所以原型下的循环依赖直接不处理。
1.  **从parentBeanFactory获取bean**
如果当前容器中没找到，则从父容器中递归找
1.  **标记Bean**
标记为已经或即将创建
1.  **获取合并Bdf**
根据beanName获取相应的bdf，同时对有父类的bean进行属性合并，成为合并后的最终bdf
1.  **处理依赖的Beans**
对于依赖的Bean，进行优先加载。所以初始化Bean会优先初始化其依赖的Bean。

若出现循环依赖，则抛异常
1.  **根据类型，初始化最终Bean实例**
不同Socpe，初始化方式有所不同
1.  **转换为需求类型**
