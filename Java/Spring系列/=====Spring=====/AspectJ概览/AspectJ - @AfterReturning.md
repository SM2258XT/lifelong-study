---
title: AspectJ - @AfterReturning
updated: 2021-04-04T20:50:56
created: 2021-04-04T17:02:57
---

执行时间：在目标方法执行完后，执行该切面方法。

目的：主要用于获取目标方法返回值，并进行处理。

在切面方法中，对拿到的返回值进行修改，不会对原方法的返回值产生影响！！！

1.  配置2个属性
    1.  value：填写execution切入点表达式
    2.  returning：名字必须和切面方法中的参数相同！
2.  获取返回值
@AfterReturning(value = "execution(public Integer doOther(..))",returning = "money")

public void afterReturning(JoinPoint jp, Integer money) {

Object\[\] objs = jp.getArgs();

money -= 1500;

System.out.println(objs\[0\] + " 实际涨的工资为 " + money);

}

特别注意：

后置通知相当于执行：

Integer money = doOther(); //通过调用方法获取返回值

此时，原方法已经执行完毕，因此获取到的返回值实际上只是一份拷贝，在切面方法中修改并不会产生影响。

