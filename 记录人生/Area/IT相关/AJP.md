---
date: 2026-05-20
---

AJP 是 Apache 和 Tomcat 之间常用的内部转发协议。

与[[HTTP]]协议间的区别在于使用范围

HTTP 是浏览器和 Web 服务器之间常用的通用协议。

`Apache` 通过以下形式表示转发到的后端地址

```
ProxyPass /app ajp://localhost:8009/app
ProxyPass /app http://localhost:8080/app
```