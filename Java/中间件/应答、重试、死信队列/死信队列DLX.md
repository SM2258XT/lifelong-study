---
title: 死信队列DLX
updated: 2022-10-04T08:26:44
created: 2022-10-02T10:25:38
---

在消费失败到达最大次数后，会自动抛出 AmqpRejectAndDontRequeueException 异常并结束重试
也就是说，想要主动结束重试，可以直接抛异常。
@Component
public class Demo05Config {
// 创建普通队列
@Bean
public Queue demo5Queue() {
return QueueBuilder.durable("d5q").exclusive().autoDelete()
.deadLetterExchange("d5e") // 当消息成为死信后，转发到该交换机
.deadLetterRoutingKey("**d5dlx_key**") // 转发时，使用该路由键。注意一定要为**死信队列绑定键**
.build();
}
// 死信队列 = 普通队列 + BindingKey
@Bean
public Queue demo5DeadQueue() {
return new Queue("d5dlx", true, false, false);
}
// 创建 Direct Exchange
@Bean
public DirectExchange demo5Exchange() {
return new DirectExchange("d5e", true, false);
}
// 普通队列绑定
@Bean
public Binding demo07Binding() {
return BindingBuilder.bind(demo5Queue()).to(demo5Exchange()).with("d5q_key");
}
// 死信队列绑定
@Bean
public Binding demo07DeadBinding() {
return BindingBuilder.bind(demo5DeadQueue()).to(demo5Exchange()).with("d5dlx_key");
}
}

spring:
rabbitmq:
host: 127.0.0.1
port: 5672
username: guest
password: guest
listener:
simple:
\# 对应 RabbitProperties.ListenerRetry 类
retry:
enabled: true
max-attempts: 3 \# 一共重试次数，包括首次正常消费。
initial-interval: 1000 \# 重试间隔，单位为毫秒。默认为 1000 。

添加 spring.rabbitmq.listener.simple.retry.multiplier 配置项来实现递乘的时间间隔，添加 spring.rabbitmq.listener.simple.retry.max-interval 配置项来实现最大的时间间隔。
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)
