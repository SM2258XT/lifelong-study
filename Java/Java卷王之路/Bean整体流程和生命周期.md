---
title: Bean整体流程和生命周期
updated: 2022-10-30T09:36:51
created: 2022-10-30T09:15:54
---

Bean整体流程和生命周期
2022年10月30日
9:15

**Bean创建流程**
1.  **定位资源**
IOC容器的第一步就是定位资源
1.  **装载和解析bdf**
将用户书写的Bean，表示成IOC容器内部数据结构bdf

bdfResourceLoader读取并解析用户编写的xml，解析每个bean标签等，成为最初的bdf。
1.  **注册 / 加载bdf**
注册到BeanFactory的bdfMap（HashMap）和bdfNames（ArrayList）中

注意：此时**还没有开始 DI** （Bean创建），只会发生在第一次向IOC容器索取Bean时才开始初始化/创建Bean。（因为BeanFactory默认是懒汉式）
1.  **初始化 / 创建bdf**
调用BeanFactory#**getBean()**时，才会触发Bean初始化。

若该Bean已经初始化完成了，则直接返回，否则进行实例化、**依赖注入DI**、装配并返回等一系列操作

**Bean生命周期**

bean 生命周期有七步 （正常生命周期为五步，而配置后置处理器后为七步）
1.  通过构造器创建 bean 实例（无参数**构造**）
2.  为 bean 的属性设置值和对其他 bean 引用（调用**set**方法）
3.  把 bean 实例传递 bean 前置处理器的方法 postProcessBeforeInitialization
4.  调用 bean 的初始化的方法（需要进行配置**初始化**的方法）
5.  把 bean 实例传递 bean 后置处理器的方法 postProcessAfterInitialization
6.  bean 可以使用了（对象**获取**到了）
7.  当容器关闭时候，调用 bean 的销毁的方法（需要进行配置**销毁**的方法）
