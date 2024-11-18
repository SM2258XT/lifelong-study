---
title: SecurityConfig
updated: 2021-08-08T16:53:19
created: 2021-08-08T16:41:06
---

@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig extends WebSecurityConfigurerAdapter {
@Autowired
@Qualifier("myUserDetailService")
UserDetailsService userDetailsService;
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
auth.userDetailsService(userDetailsService).passwordEncoder(new BCryptPasswordEncoder());
}
@Bean
public PasswordEncoder passwordEncoder() {
return new BCryptPasswordEncoder();
}
}
1.  @AutoWired注解使用的注入方式为byType，因此同一类型的bean存在多个时无法注入，需要使用@Qualifier注解指定beanName。
解决：

@Autowired

@Qualifier("myUserDetailService")

UserDetailsService userDetailsService;
