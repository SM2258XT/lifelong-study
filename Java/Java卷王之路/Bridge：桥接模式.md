---
title: Bridge：桥接模式
updated: 2022-10-25T10:46:10
created: 2022-10-23T17:01:57
---

![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)
图形有3种，颜色有3种
一共3\*3=9种模式，是不是要写9个类呢？
如果再引入一种颜色，是不是又要多写3个类呢？
使用桥接模式，提供两个父类一个是颜色、一个形状，颜色父类和形状父类两个类都包含了相应的子类，然后根据需要对颜色和形状进行组合。

**概述：**
将抽象部分与它的实现部分分离开来，使他们都可以独立变化。

public class Client {
public static void main(String\[\] args) {
//白色
Color white = new White();
//正方形
Shape square = new Square();
//白色的正方形
square.setColor(white);
square.draw();

//长方形
Shape rectange = new Rectangle();
rectange.setColor(white);
rectange.draw();
}
}
