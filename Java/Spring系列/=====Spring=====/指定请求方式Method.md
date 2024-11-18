---
title: 指定请求方式Method
updated: 2021-07-06T17:32:53
created: 2021-04-19T08:47:38
---

指定请求方式Method
2021年4月19日
8:47

springmvc可以控制请求方式，通过@RequestMapping注解的属性method来实现。
使用：
@RequestMapping(value = {"/some.do","/other.do"},method = {RequestMethod.GET,RequestMethod.POST})

public ModelAndView doSome(){

......

}

