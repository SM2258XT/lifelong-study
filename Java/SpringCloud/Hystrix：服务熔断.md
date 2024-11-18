---
title: Hystrix：服务熔断
updated: 2021-10-04T16:33:06
created: 2021-10-04T15:58:35
---

1.  服务熔断：直接拒绝访问！
不再去访问生产者8001，而是直接调用降级方法返回结果。

服务降级 -\> 熔断 -\> 恢复调用链路
1.  熔断类型
熔断打开：请求不再进行调用当前服务，内部设置时钟一般为MTTR(平均故障处理时间)，当打开时长达到所设时钟则进入半熔断状态。

熔断关闭：熔断关闭不会对服务进行熔断。

熔断半开：部分请求根据规则调用当前服务，如果请求成功且符合规则则认为当前服务恢复正常，关闭熔断。
1.  熔断的三个重要参数
快照时间窗：

断路器确定是否打开需要统计一些请求和错误数据，而统计的时间范围就是快照时间窗，默认为最近的10秒。

请求总数阀值：

在快照时间窗内，必须满足请求总数阀值才有资格熔断。默认为20，意味着在10秒内，如果该hystrix命令的调用次数不足20次7，即使所有的请求都超时或其他原因失败，断路器都不会打开。

错误百分比阀值：

当请求总数在快照时间窗内超过了阀值，比如发生了30次调用，如果在这30次调用中，有15次发生了超时异常，也就是超过50%的错误百分比，在默认设定50%阀值情况下，这时候就会将断路器打开。
1.  断路器开启或关闭的条件
到达以下阀值，断路器将会开启：

当满足一定的阀值的时候（默认10秒内超过20个请求次数)

当失败率达到一定的时候（默认10秒内超过50%的请求失败)

当开启的时候，所有请求都不会进行转发

一段时间之后（默认是5秒)，这个时候断路器是半开状态，会让其中一个请求进行转发。如果成功，断路器会关闭，若失败，继续开启。
1.  使用Hystrix配置服务熔断
@HystrixCommand(fallbackMethod = "paymentCircuitBreaker_fallback",commandProperties = {

@HystrixProperty(name = "circuitBreaker.enabled",value = "true"),// 是否开启断路器

@HystrixProperty(name = "circuitBreaker.requestVolumeThreshold",value = "10"),// 请求次数

@HystrixProperty(name = "circuitBreaker.sleepWindowInMilliseconds",value = "10000"), // 时间窗口期

@HystrixProperty(name = "circuitBreaker.errorThresholdPercentage",value = "60"),// 失败率达到多少后跳闸

})

public String paymentCircuitBreaker(@PathVariable("id") Integer id) {

...

}

