# MCP 与内网环境：MCP Server 默认就是本地的

用户在第 6 课后提出关键问题：内网环境无法连接外网 MCP server，还能用非内置的 MCP 工具吗？

答案是可以。用户混淆了 MCP server 的部署位置——MCP server 默认运行在本地（localhost），不需要外网。这是一个重要的概念澄清。

Evidence：用户的 MCP 笔记中描述 MCP 为「统一工具接入协议」——这个理解正确但不完整，缺少对 MCP 部署架构（本地运行）的认知。
