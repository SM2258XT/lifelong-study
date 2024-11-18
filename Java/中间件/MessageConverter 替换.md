---
title: MessageConverter 替换
updated: 2022-10-03T14:44:49
created: 2022-10-03T08:42:57
---

MessageConverter 替换
2022年10月3日
8:42

**为什么不使用默认Converter**
默认情况下，RabbitTemplate 采用 SimpleMessageConverter，其内部采用 Java 自带序列化方式，实现对 Java POJO 对象的序列化和反序列化，所以官方目前不是很推荐。主要缺点：1）无法跨语言、2）序列化后的字节数组太大、3）序列化性能太低
因此一般情况下，我们建议采用 Jackson2JsonMessageConverter ，使用 **JSON** 实现对 Java POJO 对象的序列化和反序列化。

**Jackson如何序列化**
在序列化时，会在 RabbitMQ 消息 MessageProperties 的 \_\_TypeId\_\_ 上，填写值为 Message 消息对应的类全名。
在反序列化时，会根据 RabbitMQ 消息 MessageProperties 的 \_\_TypeId\_\_ 的值，反序列化消息内容成该 Message 对象。

\<dependency\>
\<groupId\>com.fasterxml.jackson.core\</groupId\>
\<artifactId\>jackson-databind\</artifactId\>
\<version\>2.9.10.1\</version\>
\</dependency\>

创建 Jackson2JsonMessageConverter Bean 对象，RabbitAutoConfiguration.RabbitTemplateConfiguration 在创建 RabbitTemplate Bean 时，会自动注入它。
@Bean
public MessageConverter messageConverter() {
return new Jackson2JsonMessageConverter();
}

