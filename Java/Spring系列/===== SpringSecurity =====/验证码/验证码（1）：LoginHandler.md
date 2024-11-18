---
title: 验证码（1）：LoginHandler
updated: 2021-08-13T17:14:45
created: 2021-08-12T09:35:19
---

验证码（1）：LoginHandler
2021年8月12日
9:35

@Component
public class MyAjaxLoginSuccessHandler implements AuthenticationSuccessHandler {
@Override
public void onAuthenticationSuccess(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Authentication authentication) throws IOException, ServletException {
PrintWriter pw = httpServletResponse.getWriter();
CaptchaResponseModel model = new CaptchaResponseModel(0, "success!");
ObjectMapper mapper = new ObjectMapper();
String json = mapper.writeValueAsString(model);
System.out.println("====================");
System.out.println(json);
pw.write(json);
}
}
import com.fasterxml.jackson.databind.ObjectMapper;
import com.zhang.crm.vo.CaptchaResponseModel;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.authentication.AuthenticationFailureHandler;
import org.springframework.stereotype.Component;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@Component
public class MyAjaxLoginFailureHandler implements AuthenticationFailureHandler {
private boolean captchaCheckedFailed = false;
public MyAjaxLoginFailureHandler() {
}
public void setCaptchaVerifyFailed() {
this.captchaCheckedFailed = true;
}
@Override
public void onAuthenticationFailure(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, AuthenticationException e) throws IOException, ServletException {
PrintWriter pw = httpServletResponse.getWriter();
CaptchaResponseModel model = null;
if (captchaCheckedFailed)
model = new CaptchaResponseModel(2, "captcha error!");
else
model = new CaptchaResponseModel(1, "account error!");
ObjectMapper mapper = new ObjectMapper();
String json = mapper.writeValueAsString(model);
System.out.println(json);
pw.write(json);
}
}
