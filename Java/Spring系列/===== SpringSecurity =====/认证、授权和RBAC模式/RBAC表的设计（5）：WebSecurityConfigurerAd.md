---
title: RBAC表的设计（5）：WebSecurityConfigurerAdapter
updated: 2021-08-10T06:39:49
created: 2021-08-09T17:09:44
---

@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class CustomSecurityConfig extends WebSecurityConfigurerAdapter {
@Autowired
CustomerUserDetailService userDetailService;

@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
auth.userDetailsService(userDetailService).passwordEncoder(new BCryptPasswordEncoder());
}

@Override
protected void configure(HttpSecurity http) throws Exception {
http.authorizeRequests()
.antMatchers("/public/index").permitAll()
.antMatchers("/visitor/\*\*").hasRole("visitor")
.antMatchers("/customer/\*\*").hasRole("customer")
.antMatchers("/staff/\*\*").hasRole("staff")
.antMatchers("/admin/\*\*").hasRole("admin")
.anyRequest().authenticated() //其他请求需要身份认证
.and()
.formLogin();
}
}
