---
title: 常用的Predicate
updated: 2021-10-05T10:17:45
created: 2021-10-05T09:32:01
---

常用的Predicate
2021年10月5日
9:32

spring:
application:
name: cloud-gateway

cloud:
gateway:
discovery:
locator:
enabled: true
routes:
\- id: payment_routh \#payment_route \#路由的ID，没有固定规则但要求唯一，建议配合服务名
\#uri: <http://localhost:8001> \#匹配后提供服务的路由地址
uri: lb://cloud-payment-service \#匹配后提供服务的路由地址
predicates:
\- Path=/payment/\*\* \# 断言，路径相匹配的进行路由
\- After=2017-01-20T17:42:47.789-07:00\[Asia/Shanghai\]
\- Between=2017-01-20T17:42:47.789-07:00\[America/Denver\], 2017-01-21T17:42:47.789-07:00\[America/Denver\]
