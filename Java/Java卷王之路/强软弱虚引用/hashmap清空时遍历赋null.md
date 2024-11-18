---
title: hashmap清空时遍历赋null
updated: 2022-09-15T14:28:27
created: 2022-09-15T12:17:55
---

清空方法显式指定为null
public void clear() {
Node\<K,V\>\[\] tab;
// 增加修改次数
modCount++;
if ((tab = table) != null && size \> 0) {
// 设置大小为 0
size = 0;
// 设置每个位置为 null
for (int i = 0; i \< tab.length; ++i)
tab\[i\] = null; // 因为是强引用，不释放则无法垃圾回收？？？
}
}
