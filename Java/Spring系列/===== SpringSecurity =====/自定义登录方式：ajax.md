---
title: 自定义登录方式：ajax
updated: 2021-08-12T09:28:37
created: 2021-08-10T16:11:05
---

@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class CustomSecurityConfig extends WebSecurityConfigurerAdapter {
@Autowired
CustomerUserDetailService userDetailService;
@Autowired
MyAjaxLoginSuccessHandler myAjaxLoginSuccessHandler;
@Autowired
MyAjaxLoginFailureHandler myAjaxLoginFailureHandler;
@Override
protected void configure(AuthenticationManagerBuilder auth) throws Exception {
auth.userDetailsService(userDetailService).passwordEncoder(new BCryptPasswordEncoder());
}
@Override
protected void configure(HttpSecurity http) throws Exception {
String loginPage = "/login-ajax.html";
String loginUrl = "/login.do";
//设置请求的拦截规则
http.authorizeRequests()
//静态资源全部放行
.antMatchers("/api/\*\*", "/css/\*\*", "/images/\*\*", "/js/\*\*", "/lib/\*\*").permitAll()
//登录页面和登录表单提交URL需要放行
.antMatchers(loginPage, loginUrl).permitAll()
//其他请求需要身份认证
.anyRequest().authenticated();
//设置登入登出相关信息
http.formLogin().loginPage(loginPage).loginProcessingUrl(loginUrl)
.successHandler(myAjaxLoginSuccessHandler).failureHandler(myAjaxLoginFailureHandler);
http.csrf().disable(); //关闭csrf恶意跨域攻击防护
}
}
@Component
public class MyAjaxLoginFailureHandler implements AuthenticationFailureHandler {
@Override
public void onAuthenticationFailure(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, AuthenticationException e) throws IOException, ServletException {
PrintWriter pw = httpServletResponse.getWriter();
pw.println("{\\msg\\:\\error\\}");
}
}

@Component
public class MyAjaxLoginSuccessHandler implements AuthenticationSuccessHandler {
@Override
public void onAuthenticationSuccess(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Authentication authentication) throws IOException, ServletException {
PrintWriter pw = httpServletResponse.getWriter();
pw.println("{\\msg\\:\\ok\\}");
}
}

