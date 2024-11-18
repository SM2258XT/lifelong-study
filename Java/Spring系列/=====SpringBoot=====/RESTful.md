---
title: RESTful
updated: 2021-08-26T17:53:02
created: 2021-07-09T17:08:17
---

每个参数都要用@PathVariable来获取
如果参数为自定义类对象，则不需要@@PathVariable来获取，spring会自动赋值封装。

@GetMapping("/pageTeacherCondition/{pageNum}/{pageSize}")
public R pageTeacherCondition(
@PathVariable Integer pageNum,
@PathVariable Integer pageSize,
ConditionalQuery conditionalQuery //自定义类，spring会扫描该类的属性有哪些，并把请求中符合条件的数据封装到这个类对象中作为参数。
)

