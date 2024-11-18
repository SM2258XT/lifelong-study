---
title: BeanDefinition加载解析
updated: 2022-11-11T13:56:15
created: 2022-10-29T10:55:15
---

BeanDefinition加载解析
<http://svip.iocoder.cn/Spring/IoC-load-BeanDefinitions/>
2022年10月29日
10:55

ClassPathResource resource = new ClassPathResource("bean.xml"); // 获取资源
DefaultListableBeanFactory factory = new DefaultListableBeanFactory(); // 获取 BeanFactory
XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(factory); // 从 BeanFactory 创建一个 BeanDefinitionReader 对象，作为资源的解析器
reader.**loadBeanDefinitions**(resource); // 装载资源
注意：直到这里，还没有开始Bean的注册！

**三个步骤**
1.  资源定位
比如使用外部xml资源来描述Bean对象，首先需要定位这个xml在哪
1.  装载
装载就是 BeanDefinition 的载入过程

BeanDefinitionReader 读取、解析 Resource 资源，将用户定义 Bean 表示成 IoC 容器的内部数据结构：**BeanDefinition**
- 在 IoC 容器内部维护着一个 BeanDefinition Map 的数据结构
- 在配置文件中每一个 \<bean\> 都对应着一个 BeanDefinition 对象。
1.  注册
向 IoC 容器注册已经解析好的 BeanDefinition，注入到一个HashMap中，通过 BeanDefinitionRegistry 接口来实现的。

**资源的装载和避免死循环**
XmlBeanDefinitionReader \# loadBeanDefinitions方法，将加载过的资源保存到一个HashSet中。
加载死循环：避免资源在加载时，还没有完成加载，又去加载自身
避免方式：加载前，先判断一下是否正在加载中（在不在HashSet中，存在则抛异常），若不在才继续加载，这样可以避免死循环。当加载完成后会从set中移除。
之后，执行核心方法doLoadBeanDefinitions，这个才是加载BeanDefinition的真正逻辑
