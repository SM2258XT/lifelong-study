---
title: Adapter：适配器模式
updated: 2022-10-23T15:14:13
created: 2022-10-12T16:19:15
---

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
**概述：**
米国的充电器（110V）在米国使用的很好，但是带回中国就很难使用（220V）。
为了使接口正常工作，因此引入适配器（Adapter）
**定义：**
定义一个适配器包装类，包装不兼容接口的对象。包装类去持有并调用原始类方法，同时给客户提供包装后的方法。
**可见性：**
客户只能看到目标接口。至于是谁实现的接口、原始类是什么、什么是适配器，客户都不需要知道。

**目标接口，用户可见的唯一接口：**
public interface V220Socket {
void charge220();
}
**适配器**
public class V110ToV220Adapter **implements V220Socket** {
**V110Charger v110Charger; // 持有原有类对象**

public V110ToV220Adapter(V110Charger v110Charger) {
this.v110Charger = v110Charger;
}

**// 实现（伪装成）客户需要的接口，底层去调用V110原有类**
@Override
public void charge220() {
System.out.println("开始充电...");
v110Charger.charge110();
}
}
**原有类，110V充电器，无法在220V上用，需要适配器**
public class V110Charger {
void charge110(){
System.out.println("110V充电器正在工作...");
}
}
public static void main(String\[\] args) {
V110Charger v110Charger = new V110Charger();

**// 使用适配器来包装原有类**

V220Socket v220Socket = new V110ToV220Adapter(v110Charger);

v220Socket.charge220();
}
