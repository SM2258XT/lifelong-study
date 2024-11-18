---
title: RandomAccess
updated: 2022-11-07T09:47:14
created: 2022-10-25T14:10:57
---

**RandomAccess是一个标志性空接口**
告知JVM，此类可支持快速随机访问。（比如ArrayList用for循环遍历快一些，LinkedList用迭代器遍历快一些）
- 在遍历集合的时候，推荐先检查该List是否实现了RandomAccess接口，以便让不同的集合使用更优的遍历算法。通常来说，**如果用for(i=0;i\<n;i++)循环遍历比用迭代器遍历的速度快，那么推荐实现RandomAccess接口**

**Collections集合工具类**
public static \<T\> int binarySearch(List\<? extends Comparable\<? super T\>\> list, T key) {
if (list instanceof RandomAccess \|\| list.size()\<BINARYSEARCH_THRESHOLD) // 如果需要遍历的集合，实现了RandomAccess接口，或者集合大小小于二分查找阀值时
return Collections.indexedBinarySearch(list, key); // 按**下标二分**查找，效率更好
else
return Collections.iteratorBinarySearch(list, key); // 否则使用**迭代器二分**查找效率更好
}
