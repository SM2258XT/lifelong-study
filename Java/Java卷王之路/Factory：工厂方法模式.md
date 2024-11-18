---
title: Factory：工厂方法模式
updated: 2024-09-13T19:19:24
created: 2022-10-12T16:19:17
---

**描述：**
产品和工厂各有一个顶级接口（或abstract class），工厂生产的是顶级父类产品。
另外有多个具体的产品和多个具体的工厂，分别实现顶级工厂和产品接口，在子类工厂中完成生产的具体方法。

注意：生产的具体逻辑是在子类工厂中实现的，父工厂仅仅是规定返回值必须为顶级产品。

**缺点（我总结的）**
由于实际的创建方法，延迟到了子类去实现，因此每多一种产品，就需要多一个工厂（其实也理所当然）

**错误例子：**
顶级工厂应该是interface或者abstract，生产产品并不该由顶级工厂执行。
public class SimplePizzaFactory { // 当顶级工程为class的时候，肯定错了。
public Pizza createPizza(String type) { // 试想：如果此时出一种新口味，那么就会多加一行 if -else，不符合开闭原则
Pizza pizza = null;
if (type.equals("cheese")) {
pizza = new CheesePizza();
} else if (type.equals("clam")) {
pizza = new ClamPizza();
} else if (type.equals("pepperoni")) {
pizza = new PepperoniPizza();
…..
}
}
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image2.png)
