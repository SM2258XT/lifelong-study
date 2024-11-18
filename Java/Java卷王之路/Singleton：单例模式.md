---
title: Singleton：单例模式
updated: 2024-09-13T19:19:23
created: 2022-10-12T16:19:16
---

**概述：**

有时候某些对象只需要一个，如：线程池、缓存、对话框等等，对于这类对象我们只能有一个实例，如果我们制造出多个实例，就会导致很多问题产生。

**定义：**
1.  只能有一个实例
2.  必须自行实例化
3.  必须自行提供访问点
**重点：**

多线程下的重复初始化，如何解决。

**几种初始化方式：**

重点掌握双重检查锁
1.  使用方法级synchronized
private static synchronized Singleton getInstance(){

if(singleton == null){

singleton = new Singleton();

}

return singleton;

}
1.  直接初始化static变量
private volatile static Singleton singleton = new Singleton();
1.  **双重检查锁**
private static Singleton getInstance() {

if (singleton == null) {

synchronized (Singleton.class) {

if (singleton == null) { // 确保只有第一个拿到锁的，才会执行初始化。

singleton = new Singleton();

}

}

}

return singleton;

}

