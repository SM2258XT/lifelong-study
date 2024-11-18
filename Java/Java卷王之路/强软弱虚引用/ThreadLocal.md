---
title: ThreadLocal
updated: 2022-10-25T15:31:30
created: 2022-05-02T12:01:52
---

ThreadLocal需要手动new出来。
1.  Thread类保存了两个该map类型的属性：
ThreadLocal.ThreadLocalMap threadLocals = null;

ThreadLocal.ThreadLocalMap inheritableThreadLocals = null;

初始值都为null，只有在调用local.set / get方法时，才会创建他们：t.threadLocals = new ThreadLocalMap(**this**, firstValue); //this表示当前threadlocal对象

1.  ThreadLocal类，包含了一个静态内部类ThreadLocalMap，相当于一个HashMap，内部使用的是Entry数组来保存值。
private Entry\[\] table; //保存同一个threadlocal下，所有不同线程的某个变量的副本值集合。

static class Entry extends **WeakReference**\<ThreadLocal\<?\>\> { //注意这里是弱引用，利于GC回收，但也会出内存泄漏问题

Object value;

Entry(**ThreadLocal**\<?\> **k**, Object v) {

super(k);

value = v;

}

}

**使用场景：**
1.  当线程里的数据调方法很深的时候，可以用threadlocal保存数据，避免每次都频繁调用方法
2.  springmvc一次请求中，从最前面的listener，到controller、service都是同一个线程，可以保存当次回话的用户信息
特别注意：根据原理，使用的必须是同一个threadlocal对象（ThreadLocalMap.Entry e = map.getEntry(**this**);

因此要保证listener、controller和service使用的是同一个threadlocal

**内存泄漏：**
ThreadLocal.ThreadLocalMap.Entry中使用的 key 为 ThreadLocal 的弱引用（如果这个对象只存在弱引用，那么在下一次垃圾回收的时候必然会被清理掉）

所以如果ThreadLocal没有被外部强引用的情况下，在垃圾回收的时候会被清理掉的（ThreadLocal th = xxx，这个th是强引用）

所以ThreadLocalMap中使用这个ThreadLocal的key也会被清理掉。但是，**value是强引用**，不会被清理（entry.value = val

这样一来就会出现key为null的value（value无法回收）

**实际项目中：**
使用一处公开的静态强引用来维系：public static ThreadLocal\<UserInfoTo\> toThreadLocal = new ThreadLocal\<\>();
其他地方需要调用时，应该调用同一个threadlocal对象而不是new一个：CartInterceptor.toThreadLocal.get();
当

**ThreadLocal与Synchronized的区别**
1.  Synchronized用于线程间的数据共享，而ThreadLocal则用于线程间的数据隔离。
2.  Synchronized是利用锁的机制，使变量或代码块在某一时该只能被一个线程访问。
ThreadLocal为每一个线程都提供了变量的副本，使得每个线程在某一时间访问到的并不是同一个对象，这样就隔离了多个线程对数据的数据共享。

Synchronized却正好相反，它用于在多个线程间通信时能够获得数据共享。

一句话理解：向ThreadLocal里面存东西就是向它里面的Map存东西的，然后ThreadLocal把这个Map挂到当前的线程下，这样Map就只属于这个线程了。

**ThreadLocal在父子线程间可以继承吗？**
不可以。还有一个类叫inheritableThreadLocals
这个类继承了ThreadLocal，并且重写了getMap和createMap方法，区别就是将 ThreadLocal 中的 threadLocals 换成了 inheritableThreadLocals，这两个变量都是ThreadLocalMap类型，并且都是Thread类的属性。
子线程在何时得到？
thread.init()方法中：

this.inheritableThreadLocals = ThreadLocal.createInheritedMap(parent.inheritableThreadLocals);
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
