---
title: Thread常用方法
updated: 2023-03-03T12:11:50
created: 2022-10-25T13:53:50
---

1.  **构造方法**
无参、Runnable（[包括FutureTask](onenote:#Runnable、Callable、FutureTask&section-id={CDD07D22-AD2B-45C8-A76D-8BE3375113E8}&page-id={C74D9224-8143-4692-AA5E-5FFF4E95E121}&end&base-path=https://d.docs.live.net/36a2ce0fd7a6557d/文档/Java/Java卷王之路.one)）
1.  **线程生命周期**
    1.  yield（static）
    2.  sleep（static）
    1.  interrupt、exit
详见[中断协商机制](onenote:#中断协商机制&section-id={CDD07D22-AD2B-45C8-A76D-8BE3375113E8}&page-id={21990B98-C78E-465A-AF43-AF930FEBAFA6}&end&base-path=https://d.docs.live.net/36a2ce0fd7a6557d/文档/Java/Java卷王之路.one)
1.  stop、suspend()、resumed()（全部已废弃）
1.  join
t1.join(); // 当前线程阻塞挂起，直至t1线程结束
1.  **get/set属性**
    1.  setPriority
    2.  setName
    3.  currentThread（static）
