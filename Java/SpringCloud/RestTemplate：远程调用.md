---
title: RestTemplate：远程调用
updated: 2021-10-11T07:40:17
created: 2021-10-02T08:46:29
---

RestTemplate：远程调用
2021年10月2日
8:46
RestTemplate是一种简答便捷访问restful服务的模板类，提供了多种远程访问Http服务的方法，是Spring提供用于Rest服务的客户端模板工具集。
1.  配置类（其实就是向spring容器中注入一个RestTemplate对象）
1.  搭建远程调用控制器
@Api(tags = "远程订单调用发起控制器")

@RestController

@CrossOrigin

@Slf4j

@RequestMapping("/consumer/payment")

public class OrderController {

public static final String PAYMENT_URL = "<http://localhost:8001>";

@Autowired

private RestTemplate restTemplate;

@ApiOperation("远程调用创建订单")

@GetMapping("/create")

public R create(@ApiParam("远程调用订单对象") Payment payment) {

return restTemplate.postForObject(PAYMENT_URL + "/payment/create", payment, R.class);

}

@ApiOperation("远程调用根据ID获取订单")

@PostMapping("/get/{id}")

public R getPayment(

@PathVariable Long id

) {

return restTemplate.getForObject(PAYMENT_URL + "/payment/get/" + id, R.class);

}

}
