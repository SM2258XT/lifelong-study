---
title: Nacos的使用
updated: 2021-10-05T16:07:56
created: 2021-10-05T15:16:23
---

1.  启动Nacos，访问网址：http://localhost:8848/nacos，默认账号密码都是nacos
2.  改pom
\<dependency\>

\<groupId\>com.alibaba.cloud\</groupId\>

\<artifactId\>spring-cloud-alibaba-dependencies\</artifactId\>

\<version\>2.1.0.RELEASE\</version\>

\</dependency\>

\<dependency\>

\<groupId\>com.alibaba.cloud\</groupId\>

\<artifactId\>spring-cloud-starter-alibaba-nacos-discovery\</artifactId\>

\</dependency\>
1.  写yml
server:

port: 9001

spring:

application:

name: nacos-payment-provider

cloud:

nacos:

discovery:

server-addr: localhost:8848 \#配置Nacos地址

management:

endpoints:

web:

exposure:

include: '\*'
1.  启动类：@EnableDiscoveryClient
2.  业务类：
@RestController

@RequestMapping("/payment/nacos")

public class PaymentController {

@Value("\${server.port}")

private String serverPort;

@GetMapping(value = "/get/{id}")

public String getPayment(@PathVariable("id") Integer id) {

return "nacos registry, serverPort: "+ serverPort+"\t id"+id;

}

}
1.  测试
启动项目，在nacos注册中心中可以看到已经上线成功的服务。
