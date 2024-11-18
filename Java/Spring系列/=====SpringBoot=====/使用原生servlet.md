---
title: 使用原生servlet
updated: 2021-07-09T17:47:42
created: 2021-07-09T17:35:14
---

由于没有外置的tomcat进行扫描，因此在创建servlet之后，还需要将servlet交给spring管理。

方法一：
1.  正常创建servlet
2.  在@SpringBootApplication主程序处，使用@ServletCompomentScan注解进行servlet扫描：
@SpringBootApplication

@ServletCompomentScan(basePackage = "com.xhu.servlet")

public class Application { ... }

方法二：

使用配置类注册Servlet，@Configuration
