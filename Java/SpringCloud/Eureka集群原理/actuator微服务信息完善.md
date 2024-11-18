---
title: actuator微服务信息完善
updated: 2021-10-02T16:36:47
created: 2021-10-02T16:26:00
---

eureka:
client:
register-with-eureka: true \#当前项目是服务提供者，要将自己注册进入eureka
fetch-registry: true \#是否从eureka抓取已有信息。单点无所谓，集群必须设置为true配合ribbon负载均衡
service-url:
defaultZone: [http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka](http://eureka7001.com:7001/eureka,http:/eureka7002.com:7002/eureka)
instance:
instance-id: payment8002 \#显示在eureka中的负载均衡服务名
prefer-ip-address: true \#显示出当前微服务的ip

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)

