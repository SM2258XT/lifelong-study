---
title: Scope
updated: 2022-07-04T14:35:05
created: 2022-05-03T12:58:28
---

对象在spring容器（IOC容器）中的生命周期，也可以理解为对象在spring容器中的创建方式。
1.  singleton
单一实例。（默认就是这个）
1.  prototype
每次都重新创建新对象给请求方。容器不在拥有当前对象的引用，请求方需要自己负责当前对象后继生命周期的管理工作，包括该对象的销毁。

对于那些不能共享使用的对象类型，应该将其定义的scope设为prototype。
1.  request
Spring容器会为每个HTTP请求创建一个全新的RequestPrecessor对象，当请求结束后，该对象的生命周期即告结束

request可以看做prototype的一种特例，除了场景更加具体之外，语意上差不多。
1.  session
Spring容器会为每个独立的session创建属于自己的全新的UserPreferences实例，比request scope的bean会存活更长的时间，其他的方面没区别，如java web中session的生命周期。
1.  global session
global session只有应用在基于porlet的web应用程序中才有意义，它映射到porlet的global范围的session，如果普通的servlet的web应用中使用了这个scope，容器会把它作为普通的session的scope对待。
