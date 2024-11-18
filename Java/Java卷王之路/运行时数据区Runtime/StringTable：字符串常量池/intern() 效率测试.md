---
title: intern() 效率测试
updated: 2022-09-14T09:08:12
created: 2022-05-07T21:58:09
---

/\*\*
\* 使用intern()测试执行效率：空间使用上
\* 结论：对于程序中大量存在存在的字符串，尤其其中存在很多重复字符串时，使用intern()可以节省内存空间。
\*
\*/
public class StringIntern2 {
static final int MAX_COUNT = 1000 \* 10000;
static final String\[\] arr = new String\[MAX_COUNT\];

public static void main(String\[\] args) {
Integer\[\] data = new Integer\[\]{1,2,3,4,5,6,7,8,9,10};

long start = System.currentTimeMillis();
for (int i = 0; i \< MAX_COUNT; i++) {
// arr\[i\] = new String(String.valueOf(data\[i % data.length\]));
arr\[i\] = new String(String.valueOf(data\[i % data.length\])).intern();

}
long end = System.currentTimeMillis();
System.out.println("花费的时间为：" + (end - start));

try {
Thread.sleep(1000000);
} catch (InterruptedException e) {
e.printStackTrace();
}
System.gc();
}
}
分析：
1.  直接 new String ：由于每个 String 对象都是 new 出来的，所以程序需要维护大量存放在堆空间中的 String 实例，程序内存占用也会变高
2.  使用 intern() 方法：由于数组中字符串的引用都指向字符串常量池中的字符串，所以程序需要维护的 String 对象更少，内存占用也更低

结论：
1.  对于程序中大量使用存在的字符串时，尤其存在很多已经重复的字符串时，使用intern()方法能够节省很大的内存空间。
2.  大的网站平台，需要内存中存储大量的字符串。比如社交网站，很多人都存储：北京市、海淀区等信息。这时候如果字符串都调用intern() 方法，就会很明显降低内存的大小。
