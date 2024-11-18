---
title: 集成MyBatis（2）使用
updated: 2022-06-17T16:54:53
created: 2021-07-09T08:51:47
---

集成MyBatis（2）使用
2021年7月9日
8:51

1.  在pom中复制资源拷贝代码（也可以放在resource里，推荐！）
2.  在主配置文件中设置连接数据库信息：
spring.datasource.driver-class-name=com.mysql.jdbc.Driver

spring.datasource.url=jdbc:mysql://localhost:3306/springdb

spring.datasource.username=root

spring.datasource.password=zhang

logging.level.com.example.demo.mapper=debug \#可选，开启mybatis日志，其中logging.level之后的为mapper所在包。
1.  为dao接口（mapper接口）添加注解@Mapper。
2.  创建service包，里面放服务层接口，接着创建service.impl包，里面放service实现类
3.  为service实现类声明@Service，并注入dao层对象。
@Autowired

private StudentMapper studentMapper;

如果要支持事务，则在service方法上添加注解@Transactional即可
1.  为Controller层声明@Controller，并注入service层对象
@Autowired

private StudentService studentService;

注意：

Controller需要被spring管理，因此Controller层需要使用@Controller、@RequestMapping、@ResponseBody等注解；

Controller中需要@Autowired Service，因此Service层需要使用@Service以交给spring容器管理；

Service中需要@Autowired Mapper，因此Mapper层需要使用@Mapper以交给spring容器管理

简便方法：
1.  如果不想每个dao接口都去加个@Mapper注解，则可以创建一个mapper扫描器：
在@SpringBootApplication类上，加上注解@MapperScan（basePackage = "xom.xhu.mapper"）
1.  如果不想写mapper的资源拷贝代码，则可以将mapper.xml放到resources中：
在resources中创建一个文件夹mapper并放入所有mapper.xml，在核心配置文件中指定mybatis.mapper-location=classpath:mapper/\*.xml
