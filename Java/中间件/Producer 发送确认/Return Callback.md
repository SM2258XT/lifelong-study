---
title: Return Callback
updated: 2022-10-04T08:45:39
created: 2022-10-03T14:33:31
---

概念：当 Producer 成功发送消息到 Broker，但是 Exchange 匹配不到 Queue 时，Broker 会将该消息回退给 Producer
使用：同样是使用全局设置

==@Component==
public class RabbitProducerReturnCallback implements RabbitTemplate.ReturnCallback {
public RabbitProducerReturnCallback(RabbitTemplate rabbitTemplate) {
rabbitTemplate.setReturnCallback(this);
}
@Override
public void returnedMessage(Message message, int replyCode, String replyText, String exchange, String routingKey) {
log.error("\[returnedMessage\]\[message: \[{}\] replyCode: \[{}\] replyText: \[{}\] exchange: \[{}\] routingKey: \[{}\]\]",
message, replyCode, replyText, exchange, routingKey);
}
}

**原理推测：**Mandatory 参数
**疑问：**发送“成功”的判断，到底是交换机收到消息，还是收到消息并投递到队列中？
