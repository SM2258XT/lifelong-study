---
title: springboot常用注解
updated: 2021-07-09T17:00:58
created: 2021-07-09T16:50:10
---

1.  @RestController：放在Controller类上，替代@Controller
此时该类中所有方法返回都是json对象，给每个方法都默认加上了@ResponseBody注解，只需要写@RequestMapping("/xxx")即可。
1.  @GetMapping：放在Controller方法上，替代@RequestMapping
此时该方法只支持Get方法。
1.  @PostMapping：放在Controller方法上，替代@RequestMapping
此时该方法只支持Post方法。
