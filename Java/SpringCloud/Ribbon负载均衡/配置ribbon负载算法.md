---
title: 配置ribbon负载算法
updated: 2021-10-02T22:09:06
created: 2021-10-02T19:05:09
---

1.  配置类
特别注意：ribbon配置类不能被任何componentScan扫描到！！！

在启动类的上层新建一个包，编写配置类：

@Configuration

public class RibbonConfig {

@Bean

public IRule ribbonRule(){

return new RandomRule();

}

}
1.  在启动类上使用@RibbonClient注解
@RibbonClient(name = "CLOUD-PAYMENT-SERVICE",configuration = RibbonConfig.class) //name对应的是服务提供者的服务名，class为配置类
1.  测试，打开80模块的swagger-ui，可以看到每次访问的模块都是随机的。
