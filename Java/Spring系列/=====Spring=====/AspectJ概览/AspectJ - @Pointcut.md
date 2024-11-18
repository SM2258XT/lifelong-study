---
title: AspectJ - @Pointcut
updated: 2021-04-05T09:38:33
created: 2021-04-05T09:20:35
---

AspectJ - @Pointcut
2021年4月5日
9:20
@Pointcut：定义和管理切入点
如果项目中有多个切入点表达式是重复的，就可以使用@Pointcut进行管理。
特别注意：在其他注解上使用别名时，要加上小括号。

@Pointcut使用：
1.  属性：value = "切入点表达式"。
2.  位置：在新的自定义方法上，该方法名称将作为切入点表达式的别名。

Pointcut的创建：
@Pointcut(value = "execution(\* \*..someServiceImpl.doOther(..))")

private void mypt(){

//无需任何代码，仅仅当做一个标识符而已。一般是私有的方法，因为不需要被调用。

}

在其他通知上，使用Pointcut定义的别名：
@After(value = "mypt**()**")

public void myAfter(){

......;

}

@Before(value = "mypt**()**")

public void myBefore(){

......;

}
