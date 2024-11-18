---
title: new String（）
updated: 2022-11-22T14:22:57
created: 2022-05-07T21:40:04
---

1.  会创建几个对象？（非字面量String的new） **2个**
**String str = new String("ab");**

查看字节码：

0 new \#2 \<java/lang/String\> 在堆中创建了一个 String 对象

3 dup

4 ldc \#3 \<ab\> 在字符串常量池中尝试放入 “ab”（如果之前字符串常量池中没有 “ab” 的话） ？

6 invokespecial \#4 \<java/lang/String.\<init\>\>

9 astore_1

10 return

1.  会创建几个对象？（非字面量String的拼接） 4或5或**6个**
**new String(“a”) + new String(“b”);**

查看字节码：

0 new \#2 \<java/lang/StringBuilder\> // 1

3 dup

4 invokespecial \#3 \<java/lang/StringBuilder.\<init\>\>

7 new \#4 \<java/lang/String\> // 2，新建的空壳String

10 dup

11 ldc \#5 \<a\> // 3，字符串常量池中的"a"。如果已经存在了就不算。

13 invokespecial \#6 \<java/lang/String.\<init\>\>

16 invokevirtual \#7 \<java/lang/StringBuilder.**append**\>

19 new \#4 \<java/lang/String\> // 4，新建的空壳String

22 dup

23 ldc \#8 \<b\> // 5，字符串常量池中的"b"。如果已经存在了就不算。

25 invokespecial \#6 \<java/lang/String.\<init\>\>

28 invokevirtual \#7 \<java/lang/StringBuilder.**append**\>

31 invokevirtual \#9 \<java/lang/StringBuilder.**toString**\> // 6，调用 StringBuilder的toString() 方法，会生成一个**堆中新建的**String对象（因为源码中由Arrays.copyof直接创建）

34 astore_1

35 return
**new String()无论怎样，都会在堆中创建个新对象**
**只是创建后，如果StringTable中已经存在，则使用指针指向StringTable**

String s = new String("1");
String s2 = "1";
s.intern();
System.out.println(s == s2); // false

String s3 = new String("1") + new String("1");
String s4 = "11";
s3.intern();
System.out.println(s3 == s4); // false

