---
title: Iterable、Iterator、ForEach
updated: 2022-10-25T15:21:39
created: 2022-10-25T14:23:54
---

**Iterable接口**
public interface Iterable\<T\> {
Iterator\<T\> iterator();
default void forEach(Consumer\<? super T\> action) {
…
}
default Spliterator\<T\> spliterator() {
return Spliterators.spliteratorUnknownSize(iterator(), 0);
}
}

**forEach依赖于Iterable接口返回的Iterator对象**
注意：这里的forEach是指fori的升级版，而不是函数public void forEach(Consumer\<? super E\> action)

**最多只能删除元素**
在迭代期间，可以删除元素，但是不能添加元素。

**使用版本号保证迭代正常**
ArrayList会修改版本号
在Arraylist中，有一个属性modCount表示版本号：protected transient int **modCount** = 0;

add()、remove()操作，**都会自增修改版本号**。
迭代器会检查版本号没有改变
在其对应迭代器 Itr implements Iterator\<E\> 中，大多数操作都会先检查一下版本号是否修改过，如果被修改则直接抛出ConcurrentModificationException
迭代器最多只能修改
迭代器的remove()，会更新一下原arraylist的版本号，这样就不出错了。但是不能add，
