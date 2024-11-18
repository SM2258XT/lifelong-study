---
title: Spring Cloud Stream：消息驱动
updated: 2021-10-08T22:13:36
created: 2021-10-08T22:00:03
---

1.  Stream为什么被引入？
常见MQ(消息中间件)：ActiveMQ、RabbitMQ、RocketMQ、Kafka

各种MQ的API都不一样，如果要进行切换，会消耗大量的精力去学习新的API！

有没有一种新的技术诞生，让我们不再关注具体MQ的细节，我们只需要用一种适配绑定的方式，自动的给我们在各种MQ内切换？
1.  什么是Spring Cloud Stream？
Spring Cloud Stream是一个构建消息驱动微服务的框架。应用程序通过inputs或者outputs来与Spring Cloud Stream中binder对象交互。

通过我们配置来binding(绑定)，而Spring Cloud Stream 的binder对象负责与消息中间件交互。所以，我们只需要搞清楚如何与Spring Cloud Stream交互就可以方便使用消息驱动的方式。
1.  Spring Cloud Stream支持哪些消息中间件？
RabbitMQ、Kafka

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)
