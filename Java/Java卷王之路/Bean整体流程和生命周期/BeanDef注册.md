---
title: BeanDef注册
updated: 2022-10-30T09:08:43
created: 2022-10-30T08:52:26
---

BeanDef注册
2022年10月30日
8:52

解析标签和自定义标签，全部完成后，这时的bdf（BeadDefinition）已经可以满足使用要求了，接下来的工作就是注册这些bdf

**注册bdf**
主要由BeanDefinitionReaderUtils.registerBeanDefinition()完成
注册beanName，会默认调用DefaultListable**BeanFactory**，这个就是最核心的bean注册工厂
注册alias

**DefaultListableBeanFactory注册bdf流程**
1.  最后一次检查bdf是否合法
是否为空，是否没名字
1.  bean重写覆盖判断
从bdfMap（beanDefinitionMap）中，检查是否已经注册过，并根据项目是否开启bean覆盖来决定是否继续注册。若可以覆盖则直接put进bdfMap
1.  
