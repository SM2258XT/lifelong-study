---
title: JDK、克隆、try-catch-finally
updated: 2022-09-15T13:59:15
created: 2022-05-13T15:40:03
---

1.  JDK、JRE
JDK（Java Development Kit），Java开发工具包，包含JRE

JRE（Java Runtime Environment），Java运行环境，包含两个文件夹bin和lib，bin就是JVM，lib就是JVM工作所需要的类库。

客户运行程序，只需要安装JRE就可以了
1.  字符串相关操作
记住：字符串是不可变对象，所以只要涉及**修改**，**一定会有返回值**！！！
1.  反转？
str = stringBuilder.reverse.toString()
1.  截取和替换
str = str.substring(0,3);

str = str.replace("abc","111");
1.  如何实现对象克隆？深拷贝和浅拷贝区别是什么？
    1.  实现Cloneable接口（标志性接口），重写clone方法；
    2.  实现Serializable接口，通过对象的序列化和反序列化实现拷贝，可以实现真正的深拷贝。
    3.  BeanUtils，apache和Spring都提供了bean工具，只是这都是浅拷贝。
浅拷贝：仅仅克隆基本类型变量，不克隆引用类型变量；

深拷贝：既克隆基本类型变量，又克隆引用类型变量；
1.  try-catch-finally 中，如果 catch 中 return 了，finally 还会执行吗？
finally无论怎样都会执行

在catch 中 return之前，先执行finally
1.  创建对象哪几种方式？
    1.  new
    2.  clone()
    3.  反射：Class.forName方法获取类，在调用类的newinstance（）方法
    4.  序列化反序列化

