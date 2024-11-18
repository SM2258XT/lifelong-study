---
title: 验证码（4）：CaptchaException
updated: 2021-08-12T09:52:40
created: 2021-08-12T09:41:31
---

验证码（4）：CaptchaException
2021年8月12日
9:41

public class CaptchaException extends AuthenticationException {

public CaptchaException(String msg, Throwable cause) {
super(msg, cause);
}

public CaptchaException(String msg) {
super(msg);
}

public CaptchaException(){
super("验证码错误！");
}
}
