---
title: '@ResponseBody'
updated: 2021-07-07T20:43:24
created: 2021-04-22T19:03:36
---

返回值为可以是任意Object，但不能为String对象（这样会被认为是返回视图）
1.  加入jackson依赖
2.  在配置文件中加入注解驱动\<mvc:annotation-driven\>
注解驱动实现的功能是：完成java对象到json、xml、text、二进制等数据格式的转换。默认使用的就是jackson依赖
1.  在处理器方法上加入@ResponseBody注解
框架会自动加请求头，指定文件类型contenType:application/json;

实现代码：

@RequestMapping("/testAjax.test")

@ResponseBody

public Student testAjax() {

Student student = new Student("张三", 50);

return student;

}

浏览器得到的ajax返回结果：{"name":"张三","age":50}

如果返回值为List，里面放了多个Student，则浏览器得到的结果同样是数组里面放了多个字典，每个字典有多个键值对。解析出来之后就是jsonArray里面放了多个jsonObj

注意：如果返回值强制想要String，这样的话就会与返回视图冲突，要想区分返回的String是视图还是数据，根据有没有注解@ResponseBody来确定！
