---
date: 2026-05-19
---
`httpd.conf` 是 **Apache HTTP Server 的主配置文件**。

可以理解为：

> Apache 启动时会读取 `httpd.conf`，并根据其中的配置决定自己如何工作。

也就是说，`httpd.conf` 决定了 Apache：

- 监听哪个端口
- 网站文件放在哪里
- 是否启用 HTTPS
- 加载哪些模块
- 请求是否转发给后端
- 日志输出到哪里
- 哪些目录允许访问
- 是否配置多个站点

---

