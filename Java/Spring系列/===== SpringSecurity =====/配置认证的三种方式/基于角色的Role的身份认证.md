---
title: 基于角色的Role的身份认证
updated: 2021-08-08T15:55:28
created: 2021-08-08T09:47:37
---

基于角色的Role的身份认证
2021年8月8日
9:47

基于角色的Role的身份认证：同一个用户可以充当多个角色，同时可以开启方法级别的认证。
1.  在配置类中定义角色
@Override

protected void configure(AuthenticationManagerBuilder auth) throws Exception {

PasswordEncoder pe = this.passwordEncoder();

auth.inMemoryAuthentication()

.withUser("zhangsan")

.password(pe.encode("zhangsan666"))

.roles("visitor"); //一个角色

auth.inMemoryAuthentication()

.withUser("lisi")

.password(pe.encode("lisi666"))

.roles("visitor","admin"); //充当两个角色

}
1.  启用方法级别的安全认证：@EnableGlobalMethodSecurity(prePostEnabled = true)
@Configuration

@EnableWebSecurity

@EnableGlobalMethodSecurity(prePostEnabled = true)

public class SecurityConfig extends WebSecurityConfigurerAdapter {
1.  在方法上指定认证：@PreAuthorize
@RequestMapping("/index")

@PreAuthorize("hasAnyRole('visitor','admin')")

@ResponseBody

public String index(){

return "index：'visitor','admin'";

}

以上：用户使用zhangsan账号只能访问/index，但是使用管理员账号lisi就可以访问所有页面。
@RequestMapping("/manage")
@ResponseBody
@PreAuthorize("hasRole('admin')")
public String manage(){
return "/manage：'admin'";
}
