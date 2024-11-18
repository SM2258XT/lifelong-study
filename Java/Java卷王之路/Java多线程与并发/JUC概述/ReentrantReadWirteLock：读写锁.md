---
title: ReentrantReadWirteLock：读写锁
updated: 2022-09-11T18:54:26
created: 2022-09-10T13:29:59
---

**ReentrantReadWirteLock概念：**
读写锁，分别有一个读锁和一个写锁。通过读写锁的分离，进一步提升并发性能。
读锁：共享锁，多个线程可以同时读取，也就是**并发读，这一点是关键**！
写锁：排它锁，同一时间只能有一个线程写。
**读写锁的主要特性：**
1.  **公平性**：支持公平性和非公平性。
2.  **重入性：**支持重入。读写锁最多支持 65535 个递归写入锁和 65535 个递归读取锁。
1.  **锁降级：**遵循获取写锁，再获取读锁，最后释放写锁的次序，如此写锁能够降级成为读锁。
    - 当有读锁时，写锁就不能获得；而当有写锁时，**<u>只有获得写锁的线程</u>可以继续获得读锁**外，其他线程不能获得读锁。
    - **为什么有这种规定？**
当前线程写完A后立即读，按理来说应该读到A。但是可能这个“立即”还不够快，被其他线程光速改成了B，而读到的不是预期的A。

而在写锁还未释放之前，当前线程先预防一下，获取读锁。当写锁释放后，由于读锁被当前线程占有，因此任何线程都不能写！（读写互斥！），这样就保证了刚写的数据不被修改。

**总结：悲观锁**（写写互斥、读写互斥），但是读读并发共享

ReentrantReadWriteLock 与 ReentrantLock一样，其锁主体也是 Sync，它的读锁、写锁都是通过 Sync 来实现的。所以 ReentrantReadWriteLock 实际上只有一个锁，只是在获取读取锁和写入锁的方式上不一样。
**缺点：**
1.  线程饥饿。但是可以设置为公平锁来缓解。

**错误演示：模拟两组线程对map同时读写产生的问题**
class MyCache {
private Map\<String, Object\> map = new HashMap\<\>();
public void put(String key, Object val) {
System.out.println(Thread.currentThread().getName() + "正在写（还未写完） val：" + val);
try {
Thread.sleep(400);
} catch (InterruptedException e) {
throw new RuntimeException(e);
}
map.put(key, val);
System.out.println(Thread.currentThread().getName() + "写完了！");
}

public Object get(String key) {
Object result = null;
System.out.println(Thread.currentThread().getName() + "正在读取 key：" + key);
try {
Thread.sleep(400);
} catch (InterruptedException e) {
throw new RuntimeException(e);
}
result = map.get(key);
System.out.println(Thread.currentThread().getName() + "读取到了 val：" + result);
return result;
}
}

public class TestReadWriteLock {
public static void main(String\[\] args) {
MyCache myCache = new MyCache();
// 存数据线程组
for (int i = 0; i \< 6; i++) {
int finalI = i;
new Thread(() -\> {
myCache.put(finalI + "", finalI);
}, i + "").start();
}
// 取数据线程组
for (int i = 0; i \< 6; i++) {
int finalI = i;
new Thread(() -\> {
myCache.get(finalI + "");
}, i + "").start();
}
}
}
**存在的隐患：读写同时进行导致错误**
0正在写（还未写完） val：0
0正在读取 key：0
0读取到了 val：null
0写完了！
以上：还未写完，另一个线程同时开始读取，读到的就会是null！

**解决：使用ReentrantWriteLock**
读写锁是成对使用的，这里拿Read举例：
readWriteLock.writeLock().lock(); // 读锁
try {
…
} catch (Exception e) {
…
} finally {
readWriteLock.writeLock().unlock(); // 释放读锁
}
}

**写锁降级为读锁例子：**
ReentrantReadWriteLock readWriteLock = new ReentrantReadWriteLock();
new Thread(() -\> {
final String str = "AAA";
readWriteLock.writeLock().lock();
word = str;
System.out.println("【写线程】开始写：" + word);
readWriteLock.readLock().lock(); **// 还没释放写锁，立即获取读锁。如果不获取读锁，则数据会变成其他的**
readWriteLock.writeLock().unlock();
try {
Thread.sleep(800);
} catch (InterruptedException e) {
throw new RuntimeException(e);
}

System.out.println("【写线程】写完了，立即读：" + word + "\t（预期读到：" + str);
readWriteLock.readLock().unlock();
}).start();

new Thread(() -\> {
final String str = "我是啦啦啦";
System.out.println("【其他线程】等待写：" + str);
readWriteLock.writeLock().lock();
word = str;
readWriteLock.writeLock().unlock();
System.out.println("【其他线程】写完成：" + word);
}).start();
}

