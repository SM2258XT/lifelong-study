---
title: Slot：局部变量表基本单位
updated: 2022-05-13T20:17:08
created: 2022-05-05T08:00:54
---

Slot：局部变量表基本单位
2022年5月5日
8:00

1.  参数值的存放总是从局部变量数组索引0的位置开始，到数组长度-1的索引结束。
2.  局部变量表，最基本的存储单元是Slot（变量槽），局部变量表中存放编译期可知的各种基本数据类型（8种），引用类型（reference），returnAddress类型的变量。
3.  在局部变量表里，32位以内的类型只占用一个slot（包括returnAddress类型），64位的类型占用两个slot（long和double）。
- byte、short、char在储存前被转换为int，boolean也被转换为int，0表示false，非0表示true
- long和double则占据两个slot
1.  JVM会为局部变量表中的每一个Slot都分配一个访问索引，通过这个索引即可成功访问到局部变量表中指定的局部变量值
2.  当一个实例方法被调用的时候，它的方法参数和方法体内部定义的局部变量将会**按照顺序被复制**到局部变量表中的每一个slot上
3.  如果需要访问局部变量表中一个64bit的局部变量值时，只需要使用前一个索引即可。（比如：访问long或double类型变量）
4.  如果当前帧是由构造方法或者实例方法创建的，那么该对象引用this将会存放在index为0的slot处，其余的参数按照参数表顺序继续排列。（this也相当于一个变量）
这也解释了为什么static方法不能用this，因为其局部变量表中根本没有this这个变量

为什么类静态
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
