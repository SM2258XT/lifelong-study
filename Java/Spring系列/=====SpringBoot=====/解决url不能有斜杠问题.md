---
title: 解决url不能有斜杠问题
updated: 2021-09-09T18:58:49
created: 2021-09-09T18:56:42
---

1.  将系统配置写在启动类run方法开始之前
System.setProperty("org.apache.tomcat.util.buf.UDecoder.ALLOW_ENCODED_SLASH", "true");

SpringApplication.run(ObjectStoreServerApplication.class, args);
1.  创建配置类
@Configuration

public class SpringbootConfig extends WebMvcConfigurerAdapter {

@Override

public void configurePathMatch(PathMatchConfigurer configurer) {

UrlPathHelper urlPathHelper = new UrlPathHelper();

urlPathHelper.setUrlDecode(false);

configurer.setUrlPathHelper(urlPathHelper);

}

}

