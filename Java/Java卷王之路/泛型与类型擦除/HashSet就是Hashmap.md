---
title: HashSet就是Hashmap
updated: 2022-09-15T14:30:16
created: 2022-09-15T07:40:20
---

HashSet就是Hashmap
2022年9月15日
7:40

- HashSet底层其实是维护的一个HashMap：
private transient HashMap\<E,Object\> map;

private static final Object PRESENT = new Object(); //这个static object作为map的虚拟value，占位值
- 调用链：hashset.put(k) -\> hashmap.put(k,v) -\> hashmap.putVal(hash(k), k, v, false, true);
- 重点掌握：hashmap.putVal(hash(k), k, v, false, true);
