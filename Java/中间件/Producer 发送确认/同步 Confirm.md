---
title: 同步 Confirm
updated: 2022-10-03T14:24:49
created: 2022-10-03T13:27:57
---

**1.手动配置同步Confirm（默认为None）**
spring:
rabbitmq:
listener:
simple:
\# acknowledge-mode: manual \# 开发者手动 ACK （Consumer）
**publisher-confirm-type: simple** \# 同步Confirm （Producer）

SIMPLE（同步Confirm）模式下，Spring-AMQP 在创建完 RabbitMQ Channel 之后，会自动调用 Channel#confirmSelect() 方法，将 Channel 设置为 Confirm 模式。

**2.核心：rabbitTemplate.invoke(action, acks, nacks) // 发送消息、成功回调、失败回调**
注意顺序：1.发送完成 -\> 2.Broker投至Queue并响应成功 -\> 3.触发回调 -\> 4.waitForConfirms阻塞结束。（先是回调，再是阻塞结束！！！）
public void send(String msg) {
rabbitTemplate.invoke(new RabbitOperations.OperationsCallback\<Object\>() {
@Override
public Object doInRabbit(RabbitOperations rabbitOperations) {
rabbitOperations.convertAndSend("d9e", "d9_key", msg); // 同步发送消息
log.info("\[doInRabbit\]\[发送消息完成\]");
rabbitOperations.**waitForConfirms**(0); // 阻塞等待确认，在回调之后结束阻塞。等待时间为0表示无限等待
log.info("\[doInRabbit\]\[等待 Confirm 完成\]");
return null;
}
}, new ConfirmCallback() { // 成功回调
@Override
public void handle(long deliveryTag, boolean multiple) throws IOException {
log.info("\[handle\]\[Confirm 成功\]");
}
}, new ConfirmCallback() { // 失败回调，包括Broker自身故障
@Override
public void handle(long deliveryTag, boolean multiple) throws IOException {
log.info("\[handle\]\[Confirm 失败\]");
}
});
}

**小提示：invoke()建议写成Lambda形式**
rabbitTemplate.invoke(rabbitOperations -\> {
rabbitOperations.convertAndSend("d9e", "d9_key", msg);
log.info("\[doInRabbit\]\[发送消息完成\]");
rabbitOperations.waitForConfirms(0);
log.info("\[doInRabbit\]\[等待 Confirm 完成\]");
return null;
},
(deliveryTag, multiple) -\> log.info("\[handle\]\[Confirm 成功\]"),
(deliveryTag, multiple) -\> log.info("\[handle\]\[Confirm 失败\]")
);

