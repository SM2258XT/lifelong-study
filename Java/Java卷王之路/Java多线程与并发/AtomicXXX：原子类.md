---
title: AtomicXXX：原子类
updated: 2022-09-16T11:01:42
created: 2022-09-12T10:06:58
---

**AtomicXXX更像是一个包装类**
是对Unsafe类的包装，正确合理的使用CAS！而不是让程序员去操作Unsafe实现CAS。
volatile和CAS二者相结合，共同保证简单变量在多线程环境下的可见性和原子性，这个专属的类就是原子变量类：AtomicXxx

**部分源码分析**
private volatile int value; // 保证可见性

do {
var5 = this.getIntVolatile(var1, var2);
} while(!this.compareAndSwapInt(var1, var2, var5, var4)); // **自旋CAS**为了保证原子性。注意：这里和锁没有关系，别搞混了！

**结论：**CAS必然与自旋一起使用，因为要反复比较，因此缺点1：消耗性能。缺点2：ABA问题。

**AtomicReference：自定义原子 包装类**
User z3 = new User("z3",24);
User li4 = new User("li4",26);
AtomicReference\<User\> atomicReferenceUser = new AtomicReference\<\>();
atomicReferenceUser.set(z3);
System.out.println(atomicReferenceUser.compareAndSet(z3,li4)+"\t"+atomicReferenceUser.get().toString());
System.out.println(atomicReferenceUser.compareAndSet(z3,li4)+"\t"+atomicReferenceUser.get().toString());
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)
