---
title: 安装rabbitmq
updated: 2022-03-29T11:49:25
created: 2022-02-01T18:05:14
---

1.  下载安装erlang，至少21.3版本
2.  配置环境变量，指向bin目录
3.  下载安装rabbitmq，切换到sbin目录：
rabbitmq-plugins enable rabbitmq_management \#启动管理后台的插件、开启这个插件才能通过浏览器访问登录页面

rabbitmq-server start \#启动rabbitmq

<http://localhost:15672> \#访问登录页面 用户名和密码都是guest
