---
title: AspectJ - Point连接点参数
updated: 2021-09-27T22:33:09
created: 2021-04-04T16:49:17
---

JoinPoint - 连接点
JoinPoint类封装了目标方法执行时的相关信息（方法名称、参数等）
JoinPoint类可以作为切面方法的参数，以此在切面方法中获取目标方法的参数等信息。
如果要使用JoinPoint，则只能放在切面方法的第一个参数位置！

@After("execution(public void doSome(..))")
public void afterGiveMoney(JoinPoint jp) {
System.out.println("切面功能：穷哭了！");

System.out.println("方法名称："+jp.getSignature().getName());

System.out.print("方法参数列表：");

Object\[\] objs = jp.getArgs();

for (Object obj : objs) {

System.out.print(obj + " ");

}
}

