---
title: '*@SentinelResource'
updated: 2022-03-28T20:08:55
created: 2022-02-02T11:02:09
---

1.  Controller里使用@SentinelResource指定资源和兜底类和兜底方法在哪
@GetMapping("/rateLimit/customerBlockHandler")

@SentinelResource(value = "customerBlockHandler",

blockHandlerClass = CustomerBlockHandler.class, //自定义限流处理类

blockHandler = "handlerException2") //类里面哪个方法

public String customerBlockHandler()

{

return "按客户自定义限流测试OK";

}
1.  创建自定义的兜底方法类，类名和方法名将被@SentinelResource引用
public class CustomerBlockHandler {

public static String handlerException1(BlockException blockException){

return "客户自定义的全局兜底异常-------1";

}

public static String handlerException2(BlockException blockException){

return "客户自定义的全局兜底异常-------2";

}

}
