---
title: volatile可见性案例
updated: 2022-09-16T11:02:24
created: 2022-05-02T16:10:21
---

结果永远不会输出《有点东西》
public class TestVolatile {
public static void main(String\[\] args) {
AAA aaa = new AAA();
aaa.start();
for (;;) {
if (aaa.isFlag()) {
System.out.println("有点东西");
}
}
}
}

class AAA extends Thread {
private boolean flag = false;

public boolean isFlag() {
return flag;
}

@Override
public void run() {
try {
Thread.sleep(1000);
} catch (InterruptedException e) {
e.printStackTrace();
}
flag = true; //睡眠1秒后，flag为true，主线程里按理来说应该输出《有点东西》
System.out.println("flag = " + flag); //验证一下flag是否为true
}
}
解决：
1.  加锁（任意锁都行）
因为某一个线程进入synchronized代码块前后，线程会获得锁，**清空工作内存**，从主内存拷贝共享变量最新的值到工作内存成为副本，执行代码，将修改后的副本的值刷新回主内存中，线程释放锁。

synchronized (TestVolatile.class) {

if (aaa.isFlag()) {

System.out.println("有点东西");

}

}
1.  Volatile修饰共享变量
每个线程操作数据的时候会把数据从主内存读取到自己的工作内存，如果他操作了数据并且写会了，他其他已经读取的线程的变量副本就会失效了，需要都数据进行操作又要再次去主内存中读取了。

volatile保证不同线程对共享变量操作的可见性，也就是说一个线程修改了volatile修饰的变量，当修改写回主内存时，另外一个线程立即看到最新的值。

