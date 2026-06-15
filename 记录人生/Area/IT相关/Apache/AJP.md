---
date: 2026-05-20
---

AJP（Apache JServ Protocol）是 [[Apache]] 和 Tomcat 等 Servlet Container 之间常用的内部转发协议。

AJP 常用于 Apache 作为 [[反向代理]] 时，将请求转发任何支持对应协议的后端服务器
```
Apache → Tomcat
Apache → WildFly
Apache → Spring Boot 内置 Tomcat
Apache → WebLogic
Apache → 其他后端服务
```
与 [[HTTP]] 的主要区别在于：

- [[HTTP]] 是通用的 Web 通信协议，常用于浏览器和 Web 服务器之间，也可用于服务器之间通信。
- AJP 主要用于 Web Server 和 Servlet Container 之间的内部通信。
- AJP 是二进制协议，HTTP 通常是文本协议。
- AJP 一般只应在内部网络中使用，不应直接暴露到外部。

Apache 可以通过以下形式配置转发到后端地址：

```apache
ProxyPass /app ajp://localhost:8009/app
ProxyPass /app http://localhost:8080/app

## 相关笔记

- [[Apache]] — 返回 Apache 总览
- [[反向代理]] — AJP 常用于反向代理场景
- [[HTTP]] — 与 AJP 对比的协议