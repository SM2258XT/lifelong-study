---
title: hashcode 和 equals
updated: 2023-02-22T09:36:34
created: 2022-05-13T16:14:50
---

**hashcode不是实际内存地址**
可以通过 jvm 的启动参数来配置不同的 hashcode 生成算法 -XX:hashCode=N
其中包括：自增、随机数、总为1、物理指针地址、异或值

**hashcode的本质意义是希望查找更快**
1.  物理地址和hashcode
    - hashcode是对象在hash表中的地址，**不是真实物理地址**！！
    - 对象如何得到hashcode呢？通过对象的物理地址转换成一个整数，然后该整数通过hash函数的算法就得到了hashcode。
2.  hashcode本质
机制类似于1.7中hashMap的数组+链表（实际不是！），先通过hashCode找到在数组哪一个桶中，再在当前桶中逐个equals比较。
1.  **为什么equals方法重写的话，建议也一起重写hashcode方法**？
业界规范：

equals()相等的两个对象他们的hashCode()必须要相等，也就是用equals()对比是绝对可靠的。

hashCode()相等的两个对象他们的equals()不一定相等（只是很大概率相等，但可能有哈希碰撞等），也就是hashCode()不是绝对可靠的。

**如果不重写，则违反规定**：equals相等的对象必须具有相等的哈希码

补充：
1.  没有重写hashcode，只要是一同一个对象，无论属性怎么变，hashcode都不会影响（因为完全不取决于字段）
但是重写了后，根据Objects.hash(name, age);指定的字段，即使是同一个对象，属性变了hashcode就会跟着变。
