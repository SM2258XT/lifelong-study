---
title: Consumer 消息确认
updated: 2022-10-18T15:18:13
created: 2022-10-03T08:42:33
---

Consumer 消息确认
2022年10月3日
8:42

**两种确认方式**
1.  自动：Broker发送成功即确认
Broker 只要将消息写入到 TCP Socket 中成功，就认为该消息投递成功，而无需 Consumer 手动确认。
1.  手动：Consumer手动确认
实际场景下，因为自动确认存在消息丢失的情况，所以在对可靠性有要求的场景下，我们基本采用手动确认。当然，如果允许消息有一定的丢失，对性能有更高的场景下，我们可以考虑采用自动确认。

**Spring-AMQP定义了3种消息确认的方式（AcknowledgeMode），在yml配置**
1.  NONE // Consumer 自动确认
2.  MANUAL // Consumer 手动确认，由开发者在消费逻辑中，手动进行确认。
3.  AUTO // Consumer 手动确认，在消费消息完成（包括正常返回、和抛出异常）后，由 Spring-AMQP 框架来“自动”进行确认。

4
