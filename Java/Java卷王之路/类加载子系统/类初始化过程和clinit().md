---
title: 类初始化过程和<clinit()>
updated: 2024-09-05T14:21:48
created: 2022-05-04T10:27:44
---

类初始化过程和\<clinit()\>
面试点：非法的前向引用、\<clinit()\>
2022年5月4日
10:27

==\<clinit\>()==收集顺序：static变量和代码块 \> 普通成员、初始化代码块
1.  初始化阶段就是执行类构造器方法==\<clinit\>()==的过程
2.  此方法不需定义，是javac编译器自动收集类中的所有类变量的**赋值动作**和静态代码块中的语句合并而来。
也就是说，当我们代码中包含static变量的时候，就会有clinit方法；**没有static，也就没有clinit**
1.  ==\<clinit\>()==方法中的指令按语句在源文件中出现的顺序执行
2.  ==\<clinit\>()==不同于类的构造器。（关联：构造器是虚拟机视角下的==\<init\>()==）
3.  若该类具有父类，JVM会保证子类的==\<clinit\>()==执行前，父类的==\<clinit\>()==已经执行完毕（因为子类加载一定导致父类先加载）
4.  虚拟机必须保证一个类的==\<clinit\>()==方法在多线程下被**同步加锁**
IDEA 中安装 JClassLib Bytecode viewer 插件，可以很方便的看字节码。安装过程可以自行百度

类的初始化阶段，是真正开始执行类中定义的java程序代码(字节码)并按程序员的意图去初始化类变量的过程。
更直接地说，初始化阶段就是执行类构造器\<clinit\>()方法的过程。
\<clinit\>()方法：由编译器自动收集类中的所有**类变量**的赋值动作和静态代码块**static{}**中的语句合并产生的
**收集顺序：**
由语句在源文件中出现的顺序所决定，重点就是类变量和静态代码块按源文件中定义的顺序决定执行顺序

static变量和代码块 \> 普通成员、初始化代码块 \> 构造器
面试题2：
考点：\<clinit()\>加锁
public class DeadThreadTest {
public static void main(String\[\] args) {
Runnable r = () -\> {
System.out.println(Thread.currentThread().getName() + "开始");
DeadThread dead = new DeadThread();
System.out.println(Thread.currentThread().getName() + "结束");
};

Thread t1 = new Thread(r,"线程1");
Thread t2 = new Thread(r,"线程2");

t1.start();
t2.start();
}
}

class DeadThread{
static{
if(true){
System.out.println(Thread.currentThread().getName() + "初始化当前类");
while(true);
}
}
}
输出结果：
线程2开始

线程1开始

线程2初始化当前类 / /然后程序卡死了
分析：
两个线程同时去加载 DeadThread 类，而 DeadThread 类中静态代码块中有一处死循环

先加载 DeadThread 类的线程抢到了同步锁，然后在类的静态代码块中执行死循环，而另一个线程在等待同步锁的释放

所以无论哪个线程先执行 DeadThread 类的加载，另外一个类也不会继续执行。（一个类只会被加载一次）

面试题1：
考点：1.非法前向引用 2.赋值顺序
public class ClassInitTest {
private static int num = 1;

static{
num = 2;
number = 20;
System.out.println(num);
//System.out.println(number); //报错：**非法的前向引用**。
//执行static是3初始化阶段\<clinit()\>，但是number还没初始化完
}

/\*\*
\* 1、linking之prepare: number = 0 --\> initial: 20 --\> 10
\* 2、这里因为静态代码块出现在声明变量语句前面，所以之前被准备阶段为0的number变量会
\* 首先被初始化为20，再接着被初始化成10（这也是面试时常考的问题哦）
\*
\*/
private static int number = 10;

public static void main(String\[\] args) {
System.out.println(ClassInitTest.num);//2
System.out.println(ClassInitTest.number);//10
}
}
