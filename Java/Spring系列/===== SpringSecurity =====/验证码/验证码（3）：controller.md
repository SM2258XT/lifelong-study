---
title: 验证码（3）：controller
updated: 2021-08-12T09:52:34
created: 2021-08-12T09:38:14
---

验证码（3）：controller
2021年8月12日
9:38

@RequestMapping("/captcha")
public void captcha(HttpServletRequest req, HttpServletResponse res) {
res.setHeader("Pragma", "No-cache");
res.setHeader("Cache-Control", "no-cache");
res.setDateHeader("Expires", 0);
res.setContentType("image/jpeg");
String captchaCode = CaptchaUtil.generateVerifyCode(4);
try {
HttpSession session = req.getSession();
session.setAttribute("captcha", captchaCode.toUpperCase());
OutputStream outputStream = res.getOutputStream();
CaptchaUtil.outputImage(110, 40, outputStream, captchaCode);
} catch (IOException e) {
e.printStackTrace();
}
}
