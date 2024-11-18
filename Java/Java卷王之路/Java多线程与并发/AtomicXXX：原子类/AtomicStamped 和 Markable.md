---
title: AtomicStamped 和 Markable
updated: 2022-09-12T10:47:30
created: 2022-09-12T10:43:10
---

**AtomicStampedReference**
携带版本号的引用类型原子类，可以解决ABA问题
解决修改过几次
状态戳原子引用

**AtomicMarkableReference**
原子更新带有标记位的引用类型对象
解决是否修改过 它的定义就是将状态戳简化为true\|false -- 类似一次性筷子

