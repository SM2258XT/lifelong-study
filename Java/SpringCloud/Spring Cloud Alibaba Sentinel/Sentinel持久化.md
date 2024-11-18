---
title: Sentinel持久化
updated: 2022-02-02T16:35:03
created: 2022-02-02T13:07:55
---

1.  引入依赖
\<!--SpringCloud ailibaba sentinel-datasource-nacos 后续做持久化用到--\>

\<dependency\>

\<groupId\>com.alibaba.csp\</groupId\>

\<artifactId\>sentinel-datasource-nacos\</artifactId\>

\</dependency\>
1.  Yml
spring:

application:

name: cloudalibaba-sentinel-service

cloud:

nacos:

discovery:

server-addr: localhost:8848 \#Nacos服务注册中心地址

sentinel:

transport:

dashboard: localhost:8080 \#配置Sentinel dashboard地址

port: 8719

datasource:

ds1:

nacos:

server-addr: localhost:8848

dataId: cloudalibaba-sentinel-service

groupId: DEFAULT_GROUP

data-type: json

rule-type: flow
1.  在nacos中添加配置，名称为当前服务名，注意：没有后缀！！！
\[{

"resource": "/rateLimit/customerBlockHandler",

"IimitApp": "default",

"grade": 1,

"count": 1,

"strategy": 0,

"controlBehavior": 0,

"clusterMode": false

}\]
1.  发布配置，刷新后在sentinel中就可以看到了。即使重启微服务，仍然保存有配置。
resource：资源名称；
limitApp：来源应用；
grade：阈值类型，0表示线程数, 1表示QPS；
count：单机阈值；
strategy：流控模式，0表示直接，1表示关联，2表示链路；
controlBehavior：流控效果，0表示快速失败，1表示Warm Up，2表示排队等待；
clusterMode：是否集群。
