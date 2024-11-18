---
title: 逐个接收参数@RequestParam
updated: 2021-07-06T17:38:58
created: 2021-04-19T08:54:45
---

逐个接收参数@RequestParam
2021年4月19日
8:54

1.  获取request和response，和Servlet一样。
public ModelAndView doSome(HttpRequest request, HttpResponse response, HttpSession session)
1.  直接列出所有参数。
    1.  参数名称和表单中的name**一样**：
public ModelAndView testParams(String userName,int userAge)

相当于springmvc自动调用了request.getParameter()并转型。

假如需要的参数int age前端提交不合法，则直接报错。解决方法：使用包装类接收Integer age，这样虽然为空但是不报错结束。
1.  参数名称和表单中的name**不一样**：
使用注解：@RequestParam("表单name") 来修饰参数。

public ModelAndView testParams(@RequestParam("<u>userName</u>") String abc, @RequestParam("<u>userAge</u>") int def)

@RequestParam另外一个参数：required

required用来指定网页是否必须提供该参数，如果要求提供但是没有提供的话则直接报错。

@RequestParam(value="userName",required=false)
