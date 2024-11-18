---
title: '@Valid 和 @Validated'
updated: 2024-09-03T16:15:14
created: 2022-08-15T11:06:40
---

**Bean Validation**
- Java官方提出的规范，并且已经历经 JSR303、JSR349、JSR380 三次标准的置顶。
- 只提供规范，不提供具体实现。
- 也就是说 javax.validation.constraints 包下的注解@NotNull、@NotEmpty只是个空注解，具体需要框架去实现。
- 实现 Bean Validation 规范的数据校验框架，主要有：
  1.  Hibernate Validator（99.99%情况下都用的这个）
并且附加了一些约束，org.hibernate.validator.constraints
1.  Spring Validation
在使用 Spring 的项目中，因为 Spring Validation 提供了对 Bean Validation 的内置封装支持，可以使用 ==@Validated== 注解，实现声明式校验，而无需手动调用 Bean Validation 提供的 API 方法（AOP）。

其实最终还是调用不同的 Bean Validation 的实现框架（也就是Hibernate）。

**@Valid**
是 Bean Validation 所定义，可以添加在普通方法、构造方法、方法参数、方法返回、成员变量上，表示它们需要进行约束校验。
**@Validated**
是 Spring Validation 所定义，可以添加在类、方法参数、普通方法上，表示它们需要进行约束校验。
**同时，@Validated 有 value 属性，支持分组校验**。

**根据使用场景选择：**
1.  声明式校验：@Validated
Spring Validation 仅对 @Validated 注解，实现声明式校验。
1.  分组校验：@Validated
Bean Validation 提供的 @Valid 注解，因为没有分组校验的属性，所以无法提供分组校验。此时，我们只能使用 @Validated注解。
1.  嵌套校验：
相比来说，@Valid 注解的地方，多了【成员变量】。这就导致，如果有嵌套对象的时候，只能使用 @Valid 注解。

public class User {

private String id;

==@Valid==

private UserProfile profile;

}

