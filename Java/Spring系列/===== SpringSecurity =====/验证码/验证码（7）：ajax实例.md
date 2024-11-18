---
title: 验证码（7）：ajax实例
updated: 2022-10-17T08:27:22
created: 2021-08-12T09:42:45
---

验证码（7）：ajax实例
2021年8月12日
9:42

\<div align="center"\>
账号：\<input type="text" id="username" value="root"\>\<br\>
密码：\<input type="text" id="password" value="zhang"\>\<br\>
\<button id="myBtn"\>登录\</button\>
\<a href="/index.html"\>访问主页，没登陆前应该访问失败！！！\</a\>
\</div\>

===================================================

\$(function () {
\$("#myBtn").click(function () {
let username = \$("#username").val();
let password = \$("#password").val();
\$.ajax({
url: "/login/loginTest.do",
type: "post",
dataType: "json",
data: {
"username": username,
"password": password
},
success: function (resObj) {
alert("访问成功！");
console.log(resObj);
console.log(resObj.code);
console.log(resObj.msg);
setTimeout(function () {
window.location = "/index.html"
},1000);
},
error: function (resText) {
alert("访问失败！" + resText.msg);
}
});
});
});
