---
date: 2026-06-14
tags:
  - AI
  - MCP
  - 配置
  - Claude Code
---

`mcp.json` 是指 Claude Code 中用于配置 MCP（Model Context Protocol）服务器的 JSON 文件，让 AI 通过 MCP 协议调用本地或远程工具。每个 MCP server 在本机作为子进程（stdio）或本地 HTTP 服务运行，不需要外网连接。

## 文件位置

| 层级 | 路径 | 作用范围 |
|------|------|---------|
| 用户级 | `~/.claude/mcp.json` | 所有项目 |
| 项目级 | `<项目>/.claude/mcp.json` | 仅当前项目 |

两者共存时，项目级和用户级的工具列表**合并**生效。

## 两种传输模式

### stdio 模式（零网络，最常用）

MCP client 直接启动 server 作为子进程，通过标准输入/输出通信。

```json
{
  "mcpServers": {
    "<server-name>": {
      "command": "<可执行命令>",
      "args": ["<参数1>", "<参数2>"],
      "env": {
        "<KEY>": "<value>",
        "<SECRET>": "${env:<系统环境变量名>}"
      },
      "cwd": "<工作目录>"
    }
  }
}
```

- `command`（必填）：启动 server 的命令，如 `python`、`node` 或可执行文件路径
- `args`（选填）：命令行参数数组
- `env`（选填）：环境变量，`${env:VAR}` 从当前系统环境变量读取（避免在配置文件中写明文密钥）
- `cwd`（选填）：server 进程的工作目录

### HTTP 模式（需要持久状态时）

```json
{
  "mcpServers": {
    "<server-name>": {
      "url": "http://127.0.0.1:<port>/mcp"
    }
  }
}
```

需先手动启动 server 进程，MCP client 连接到指定 URL。适合需要数据库连接池等持久状态的场景。

## 常见配置模板

**Python MCP server：**

```json
{
  "mcpServers": {
    "my-python-tools": {
      "command": "python",
      "args": ["-m", "my_mcp_server"],
      "cwd": "H:/projects/mcp-servers"
    }
  }
}
```

**Node.js MCP server：**

```json
{
  "mcpServers": {
    "my-node-tools": {
      "command": "node",
      "args": ["H:/tools/mcp-server/dist/index.js"]
    }
  }
}
```

**内网 API 封装（带认证）：**

```json
{
  "mcpServers": {
    "internal-api": {
      "command": "python",
      "args": ["H:/scripts/internal_mcp.py"],
      "env": {
        "API_TOKEN": "${env:INTERNAL_API_TOKEN}",
        "BASE_URL": "http://api.internal.company.com"
      }
    }
  }
}
```

## 内网场景说明

MCP server 默认运行在**本机**（stdio 模式零网络，HTTP 模式走 localhost）。其网络访问能力 = 用户本机的网络访问能力——你在内网能访问的（GitLab、Jira、数据库），MCP server 就能访问。全程不经过外网。

## 验证配置

配置完成后使用 `/mcp` 命令查看已加载的 MCP 工具列表。若 server 启动失败，Claude Code 会在启动时报错。

## 参考文献

- [MCP 官方文档：Transports（传输层）](https://modelcontextprotocol.io/docs/concepts/transports) — stdio 和 HTTP 传输方式的技术细节
- [MCP 官方快速入门：构建 MCP Server](https://modelcontextprotocol.io/quickstart/server) — Python / Node.js 快速开始

## 相关笔记

- [[MCP]] — MCP 协议概念
- [[Claude Code配置体系]] — Claude Code 配置体系总览
- [[Skills]] — Skill 与 MCP 的配合：Skill 管流程，MCP 管能力
- [[AI学习路线]] — 返回学习路线总览
