---
title: thymeleaf使用
updated: 2021-08-23T10:18:50
created: 2021-08-23T09:58:58
---

thymeleaf使用
2021年8月23日
9:58

thymeleaf的所有html都必须经过中央调度器转发访问！！！（controller返回视图）

1.  在templates目录下创建任意html文件，给该文件的html标签添加属性
\<html lang="en" xmlns:th="http://www.thymeleaf.org"\>
1.  编写controller，将请求的视图转发到该html，并设置一个msg属性作测试。
@RequestMapping("/test01")

public String test01(Model model) {

model.addAttribute("msg", "我是controller中设置的attribute");

return "test01"; //转发到templates下的html时，不用写后缀！！！

}
1.  在html中测试获取属性
\<h2 th:text="\${msg}"\>\</h2\>
