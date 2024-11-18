---
title: 验证码（3）：Filter
updated: 2021-08-12T09:52:28
created: 2021-08-12T09:36:35
---

验证码（3）：Filter
2021年8月12日
9:36

public class CaptchaFilter extends OncePerRequestFilter {
MyAjaxLoginFailureHandler failureHandler = new MyAjaxLoginFailureHandler();
@Override
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
//只有请求为/login.do，才检查验证码。这个uri必须与处理器里的登录处理地址相同
String uri = request.getRequestURI();
if (!"/login.do".equals(uri)) {
filterChain.doFilter(request, response);
return;
}
System.out.println("==========" + uri);

if (verifyCaptchaCorrectly(request)) {
filterChain.doFilter(request, response);
} else {
CaptchaException captchaException = new CaptchaException();
failureHandler.setCaptchaVerifyFailed();
failureHandler.onAuthenticationFailure(request, response, captchaException);
}
}
private boolean verifyCaptchaCorrectly(HttpServletRequest request) {
HttpSession session = request.getSession();
String correctCaptcha = (String) session.getAttribute("captcha");
String inputCaptcha = request.getParameter("captcha");
if (!WebUtil.notNullOrEmpty(correctCaptcha, inputCaptcha)) {
return false;
}
//验证码正确
if (correctCaptcha.equals(inputCaptcha.trim().toUpperCase())) {
session.removeAttribute("captcha");
return true;
}
return false;
}
}
