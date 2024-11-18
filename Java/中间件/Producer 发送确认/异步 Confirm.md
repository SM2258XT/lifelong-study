---
title: 异步 Confirm
updated: 2022-10-03T14:32:45
created: 2022-10-03T13:27:57
---

**1.手动配置异步Confirm（默认为None）**
spring:
rabbitmq:
listener:
simple:
**publisher-confirm-type: correlated** \# 异步Confirm （Producer）

SIMPLE（同步Confirm）模式下，Spring-AMQP 在创建完 RabbitMQ Channel 之后，会自动调用 Channel#confirmSelect() 方法，将 Channel 设置为 Confirm 模式。

**2.核心：创建全局的ProducerConfirm处理器**
==@Component==
public class RabbitProducerConfirmCallback implements RabbitTemplate.ConfirmCallback {
public RabbitProducerConfirmCallback(RabbitTemplate rabbitTemplate) {
rabbitTemplate.setConfirmCallback(this);
}
@Override
public void confirm(CorrelationData correlationData, boolean ack, String cause) {
if (ack) {
logger.info("\[confirm\]\[Confirm 成功 correlationData: {}\]", correlationData);
} else {
logger.error("\[confirm\]\[Confirm 失败 correlationData: {} cause: {}\]", correlationData, cause);
}
}
}
**3.Producer正常普通发送即可**
public void send(String msg) {
rabbitTemplate.convertAndSend("d8e", "hello.world", msg);
}
