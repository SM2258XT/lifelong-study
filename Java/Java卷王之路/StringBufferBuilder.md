---
title: String/Buffer/Builder
updated: 2022-05-14T21:14:32
created: 2022-05-02T09:40:02
---

String/Buffer/Builder
2022年5月2日
9:40

1.  String类是不可变类，即一旦一个String对象被创建以后，包含在这个对象中的字符序列是不可改变的，直至这个对象被销毁。
private final char value\[\];

private：数组只能在String本类中访问，子类和外部拿不到数组，也就无法对其修改。

final：value指针不能修改（但是指向的数组内容可以修改！而private不让任何人获取到数组，**变相的**确保了数组内容不能改变，弥补了这一空缺）

如何强制修改value\[\]？使用反射机制：

String s="Hydra";

System.out.println(s+": "+s.hashCode());

Field field = String.class.getDeclaredField("value");

field.setAccessible(true);

field.set(s,new char\[\]{'T','r','u','n','k','s'});

System.out.println(s+": "+s.hashCode());

1.  StringBuffer和StringBuilder底层都是包括两个属性：
char\[\] value; //字符序列

int count; //当前数组有多大，如果满了，则用Arrys.copyOf扩容

区别：Buffer是线程安全的，因此性能要低一些（用了synchronized方法）

1.  面试题
    1.  创建了几个对象？ <https://www.nowcoder.com/discuss/839996>
String s = new String("Hydra"); // 2个

// 2个

String s1 = "Hydra";

String s2 = new String("Hydra");
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image3.png)

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image4.png)
