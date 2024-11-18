---
title: Ribbon负载均衡
updated: 2021-10-05T09:30:29
created: 2021-10-02T19:00:15
---

1.  Ribbon本地负载均衡客户端VS Nginx服务端负载均衡区别：
Nginx是服务器负载均衡，客户端所有请求都会交给nginx，然后由nginx实现转发请求。即负载均衡是由服务端实现的。

Ribbon本地负载均衡，在调用微服务接口时候，会在注册中心上获取注册信息服务列表之后缓存到JVM本地，从而在本地实现RPC远程服务调用技术。
1.  负载均衡分类
    - 集中式LB
即在服务的消费方和提供方之间使用独立的LB设施(可以是硬件，如F5, 也可以是软件，如nginx)，由该设施负责把访问请求通过某种策略转发至服务的提供方;
1.  进程内LB
将LB逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选择出一个合适的服务器。

Ribbon就属于进程内LB，它只是一个类库，集成于消费方进程，消费方通过它来获取到服务提供方的地址。
1.  Ribbon默认自带的负载规则
lRule：根据特定算法中从服务列表中选取一个要访问的服务

RoundRobinRule 轮询

RandomRule 随机

RetryRule 先按照RoundRobinRule的策略获取服务，如果获取服务失败则在指定时间内会进行重

WeightedResponseTimeRule 对RoundRobinRule的扩展，响应速度越快的实例选择权重越大，越容易被选择

BestAvailableRule 会先过滤掉由于多次访问故障而处于断路器跳闸状态的服务，然后选择一个并发量最小的服务

AvailabilityFilteringRule 先过滤掉故障实例，再选择并发较小的实例

ZoneAvoidanceRule 默认规则,复合判断server所在区域的性能和server的可用性选择服务器

只要引入了spring-cloud-starter-netflix-eureka-client就自动引入了robbon
