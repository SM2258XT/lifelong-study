---
title: Feign：模板化http客户端
updated: 2021-10-06T10:25:11
created: 2021-10-03T08:01:00
---

Feign：模板化http客户端
2021年10月3日
8:01

1.  什么是Feign？
    - Feign旨在使编写Java Http客户端变得更容易。
    - 前面在使用Ribbon+RestTemplate时，利用RestTemplate对http请求的封装处理，形成了一套模版化的调用方法。
    - 但是在实际开发中，由于对服务依赖的调用可能不止一处，往往一个接口会被多处调用，所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用。所以，Feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义。

1.  Feign和OpenFeign的异同？
    - Feign是Spring Cloud组件中的一个轻量级RESTful的HTTP服务客户端Feign内置了Ribbon，用来做客户端负载均衡，去调用服务注册中心的服务。Feign的使用方式是：使用Feign的注解定义接口，类似于@Mapper持久层接口。调用这个接口，就可以调用服务注册中心的服务。
    - OpenFeign是Spring Cloud在Feign的基础上支持了SpringMVC的注解，如@RequesMapping等等。OpenFeign的@Feignclient可以解析SpringMVC的@RequestMapping注解下的接口，并通过动态代理的方式产生实现类，实现类中做负载均衡并调用其他服务。

