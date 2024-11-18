---
title: Hystrix-Dashboard图形化监控
updated: 2021-10-05T09:19:06
created: 2021-10-04T16:33:32
---

1.  改pom
\<dependency\>

\<groupId\>org.springframework.cloud\</groupId\>

\<artifactId\>spring-cloud-starter-netflix-hystrix-dashboard\</artifactId\>

\<version\>2.2.1.RELEASE\</version\>

\</dependency\>

\<dependency\>

\<groupId\>org.springframework.boot\</groupId\>

\<artifactId\>spring-boot-starter-actuator\</artifactId\>

\</dependency\>

\<dependency\>

\<groupId\>org.springframework.boot\</groupId\>

\<artifactId\>spring-boot-starter-web\</artifactId\>

\</dependency\>
1.  启动类注解：@EnableHystrixDashboard
2.  在被监控的项目的启动类中，加上hystrix配置：
@Bean

public ServletRegistrationBean getServlet() {

HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();

ServletRegistrationBean servletRegistrationBean = new ServletRegistrationBean(streamServlet);

servletRegistrationBean.setLoadOnStartup(1);

servletRegistrationBean.addUrlMappings("/hystrix.stream");

servletRegistrationBean.setName("HystrixMetricsStreamServlet");

return servletRegistrationBean;

}
1.  打开监控主页：http://localhost:9001/hystrix，填写地址即可进行可视化监控！
填写地址：http://localhost:8001/hystrix.stream
