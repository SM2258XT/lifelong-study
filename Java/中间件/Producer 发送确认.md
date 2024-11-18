---
title: Producer 发送确认
updated: 2022-10-04T08:22:44
created: 2022-10-03T08:42:34
---

Producer 发送确认
2022年10月3日
8:42

**Producer默认只保证发送成功**
默认情况下，Producer 发送消息**只保证**将消息写入到 TCP Socket 中成功，并**不保证**消息发送到 Broker 成功，并且**不保证**持久化消息到磁盘成功。
因此引入了发送消息确认的流程

**发送Confirm流程**
在 RabbitMQ中，Producer 采用 Confirm 模式，实现发送消息的确认机制，以保证消息发送的可靠性。实现流程如下：
1.  Producer 通过调用 Channel#**confirmSelect**() 方法，将 Channel 设置为 Confirm 模式。
2.  Producer-Channel 发送消息时，需要先通过 Channel#getNextPublishSeqNo() 方法，给发送的消息分配一个唯一的 ID 编号(seqNo 从 1 开始递增)，再发送该消息给 Broker 。
3.  Broker 接收到该消息，成功路由到相应的队列后，会发送一个包含消息的唯一编号 ( **deliveryTag** ) 的确认给 Producer。
4.  通过 seqNo 编号，将 Producer 发送消息的“请求”，和 Broker 确认消息的“响应”串联在一起。
通过这样的方式，Producer 就可以知道消息是否成功发送到 Broker 之中，保证消息发送的可靠性。不过要注意，整个执行的过程实际是**异步**，需要我们调用 **Channel#waitForConfirms()** 方法，同步阻塞等待 Broker 确认消息的“响应”。

**三种消息确认的方式分类（在yml配置）**
1.  SIMPLE, // 同步Confirm。这个“同步”是指调用 Channel#waitForConfirms() 方法同步阻塞等待 Broker 确认消息的“响应”。**Broker的响应本身一定是异步的**。
    1.  逐条同步
    2.  批量同步
2.  CORRELATED, // 异步Confirm
3.  NONE // 不使用 Confirm，**默认**

**按是否批量确认分类**
在开启 Channel#confirmSelect() 后，Channel 设置为 Confirm 模式，此时可以逐条Confirm也可以批量
1.  逐条确认
发布速度特别慢，因为没有确认发布的消息会阻塞后续消息的发布。
1.  批量确认
发布一批消息然后一起确认，可以极大地提高吞吐量

但是出错时，不知道是哪个消息出错，因此必须将整个批处理在内存暂存一下

当然这种仍然是同步的，一样阻塞消息发布，只不过吞吐量大很多
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
