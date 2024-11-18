---
title: '@RequestMapping放在哪'
updated: 2021-07-06T17:32:30
created: 2021-07-06T17:25:33
---

可以放在两个地方：

1.  放在Controller类方法上
@RequestMapping("/other.do")

public ModelAndView doSome(){......}

1.  放在Controller类上
@Controller

@RequestMapping("/test")

public class MyController{

@RequestMapping("/other.do")

public ModelAndView doSome(){......}

}

放在类上的@RequestMapping("/test")作用是指定模块名，一个Controller可以负责处理指定系列的请求，使用模块名能提高可读性。

未指定模块名时的访问url：/项目名/other.do

指定了模块名时的访问url：/项目名/test/other.do
