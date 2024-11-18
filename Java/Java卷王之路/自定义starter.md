---
title: 自定义starter
updated: 2022-10-21T12:54:04
created: 2022-09-04T08:36:40
---

自定义starter
2022年9月4日
8:36

1.  **引入starter基础库**
\<dependency\>

\<groupId\>org.springframework.boot\</groupId\>

\<artifactId\>spring-boot-starter\</artifactId\>

\<version\>2.2.2.RELEASE\</version\>

\</dependency\>
1.  **（推荐）创建配置类，读取yml**
==@ConfigurationProperties==(prefix = "senao.server")

public class SenaoServerProperties {

private static final Integer DEFAULT_PORT = 8000;

private Integer port = DEFAULT_PORT; // 默认为8000，yml配置port属性后会覆盖

…

}

注意！这个配置类不能单独使用，因为没有注入Spring容器。
1.  **创建自动配置类**
==@Configuration== // 声明配置类

==@EnableConfigurationProperties==(SenaoServerProperties.class) // 使 SenaoServerProperties 配置属性类生效

public class SenaoServerAutoConfiguration {

private Logger logger = LoggerFactory.getLogger(SenaoServerAutoConfiguration.class);

==@Bean==

==@ConditionalOnClass==(HttpServer.class) // 需要项目中存在 com.sun.net.httpserver.HttpServer 类。该类为 JDK 自带，所以一定成立。

public HttpServer httpServer(SenaoServerProperties serverProperties) throws IOException {

// 创建 HttpServer 对象，并启动

HttpServer server = HttpServer.create(new InetSocketAddress(serverProperties.getPort()), 0); // 使用配置

server.start();

logger.info("\[httpServer\]\[启动服务器成功，端口为:{}\]", serverProperties.getPort());

return server;

}

}
1.  **resources/META-INF/spring.factories指定配置类**
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\\

com.senao.starter.httpserver.SenaoServerAutoConfiguration

至此，这里starter就创建完成了，可以mvn install到本地仓库中，成为依赖直接供其他项目引入

1.  **测试使用**
直接创建一个maven空项目，只引入starter（不用starter-web，因为starter中引入的autoconfigure类包含了@SpringBootApplication等注解）

编写yml，指定port

运行即可
![](C:\Users\82609\AppData\Local\Temp\Java\pandoc/media/image1.png)
