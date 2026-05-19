---
date: 2026-05-19
---
`Apache HTTP Server` 是部署在服务器端的 Web 服务器软件，负责接收客户端/浏览器的 HTTP 请求。

- 对于静态资源请求，它可以直接返回 `HTML、CSS、JavaScript`图片等前端文件；

- 对于后端接口请求，它可以作为反向代理，将请求转发给 Spring Boot、Tomcat、WildFly 等后端服务。

因此，在很多系统结构中，Apache 位于浏览器和后端应用之间，起到入口、转发、静态资源配信、`HTTPS/HTTP` 协议处理等作用。

## 相关配置文件

[[httpd.conf]]

## 相关命令

1.查询加载模块

> httpd -M
