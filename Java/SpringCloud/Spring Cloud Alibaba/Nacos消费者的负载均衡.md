---
title: Nacos消费者的负载均衡
updated: 2022-10-17T08:28:19
created: 2021-10-05T16:08:04
---

Nacos默认继承了集成了Ribbon，可以直接做负载均衡。本例在消费者处配置负载均衡。
1.  引入依赖：在消费者方也引入spring-cloud-starter-alibaba-nacos-discover
2.  主启动：@EnableDiscoveryClient
3.  写yml：
server:

port: 83

spring:

application:

name: nacos-order-consumer

cloud:

nacos:

discovery:

server-addr: localhost:8848

service-url:

nacos-user-service: <http://nacos-payment-provider> \#消费者将要去访问的微服务名称(注册成功进nacos的微服务提供者)
1.  Ribbon配置类
@Configuration

public class RibbonConfig {

@Bean

@LoadBalanced

public RestTemplate getRestTemplate(){

return new RestTemplate();

}

}
1.  使用最原始的RestTemplate演示负载均衡效果
@RestController

@RequestMapping("/consumer/payment/nacos")

public class OrderNacosController {

@Resource

private RestTemplate restTemplate;

@Value("\${service-url.nacos-user-service}")

private String serverURL;

@GetMapping(value = "/get/{id}")

public String paymentInfo(@PathVariable("id") Long id) {

return restTemplate.getForObject(serverURL + "/payment/nacos/" + id, String.class);

}

}
1.  测试
打开83的swagger-ui访问controller，可以看到已经使用了负载均衡。（83消费者自己的负载）
