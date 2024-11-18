---
title: AspectJ - @Around
updated: 2021-04-05T09:19:32
created: 2021-04-04T17:34:26
---

环绕通知：@Around是功能最强的通知
1.  value：execution表达式
方法必须遵循以下要求：
1.  public
2.  必须有返回值，推荐使用Object
3.  必须有参数，固定参数为ProceedingJoinPoint

@Around(value = "execution(public Integer \*..SomeServiceImpl.doMany(..))")

public Object myAround(ProceedingJoinPoint pjp) {

Object res = null;

System.out.println("方法执行前...");

try {

res = pjp.proceed(); //主动去帮你调用目标方法并拿到返回值，相当于method.invoke()，也就是Object res = doMany();

} catch (Throwable throwable) {

System.out.println("myAround : 获取返回值失败！");

}

System.out.println("方法执行后...");

return (Integer) res + 10000;

}

特别注意：
1.  环绕通知的参数列表中，不能像其他通知一样填写JoinPoint jp。因为ProceedingJoinPoint pjp就是JoinPoint的继承接口。

理解：
1.  尽管测试代码中，我们是通过目标对象调用目标方法doMany()，但实际上我们接收到的"目标对象"已经被改变成了一个代理对象proxyX，所以通过它调用的目标方法doMany()，相当于变成了隐式调用切面方法myAround()。
2.  环绕通知可以看做是一个中间商，proceed()方法，相当于在切面方法内部帮你去调用目标方法，拿到返回值后再手动返回给你。
因此，环绕通知可以修改目标方法的返回值！
