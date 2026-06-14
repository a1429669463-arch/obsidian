---
date: 2026-06-14
tags:
  - Copilot
  - GitHub Copilot
  - 斜杠命令
---

GitHub Copilot 斜杠命令（Slash Commands）是指在 Copilot Chat 输入框或 Copilot CLI 终端中以 `/` 开头的快捷指令，用于解释代码、修复错误、生成测试、管理会话等操作。在不同环境（IDE Chat、CLI、GitHub.com、MSSQL 扩展）中可用的命令有所不同，键入 `/` 即可查看当前环境支持的命令列表。

---

## 一、Copilot Chat（VS Code / IDE 内嵌）

在 IDE 的 Copilot Chat 中输入 `/` 即可调用：

| 命令 | 功能 |
|------|------|
| `/explain` | 用自然语言解释选中代码的工作原理和设计意图 |
| `/fix` | 检测并修复选中代码中的 bug 或语法错误 |
| `/fixTestFailure` | 定位并修复失败的测试 |
| `/tests` | 为选中函数/类自动生成单元测试（含边界情况） |
| `/doc` | 为函数/类/模块自动生成文档注释 |
| `/optimize` | 分析运行时性能并给出优化建议 |
| `/simplify` | 简化代码结构、提升可读性 |
| `/generate` | 根据自然语言描述生成代码 |
| `/new` | 脚手架式创建新项目（Flask、React、Vue、Blazor 等） |
| `/clear` | 清除当前对话历史，重置上下文 |
| `/help` | 显示当前环境下所有可用命令的快速参考 |
| `/create-skill <描述或repo>` | 用自然语言描述需求，Copilot 自动生成 SKILL.md（VS Code 1.110+） |
| `/skills list` | 列出所有可用 Skills |
| `/skills info` | 查看指定 Skill 的详情 |
| `/skills reload` | 重新加载 Skills（创建新 Skill 后在会话中立即生效） |

> **配合上下文引用使用**：`#file`（指定文件）、`#selection`（选中代码）、`@workspace`（全项目索引）能让命令更精准。
>
> **`/create-skill` 工作流程**：输入命令 + 自然语言描述 → Copilot 自动读取 SKILL.md 规格 → 分析上下文 → 生成 `.github/skills/<name>/SKILL.md` → 确认后即可用 `/<skill-name>` 调用。

---

## 二、Copilot CLI（终端）

在终端中运行的 Copilot CLI 有独立的命令集：

### 会话管理

| 命令 | 功能 |
|------|------|
| `/clear` | 清除当前会话的对话历史 |
| `/exit` / `/quit` | 退出 Copilot CLI |
| `/session` / `/usage` | 显示当前会话指标（Session ID、时长、token 用量等） |
| `/chronicle` | **2026 新增** — 回顾和分析会话历史（子命令：`standup`、`tips`、`improve`、`search`） |
| `/compact` | **2026 新增** — 手动压缩长会话上下文 |
| `/remote` | **2026 新增** — 从 github.com 或 GitHub Mobile 远程控制 CLI 会话 |

### 目录与文件访问

| 命令 | 功能 |
|------|------|
| `/add-dir <dir>` | 将目录添加到允许访问列表 |
| `/list-dirs` | 列出当前允许访问的目录 |
| `/cwd [dir]` | 显示或更改当前工作目录 |

### 配置与模型

| 命令 | 功能 |
|------|------|
| `/model [model]` | 显示或切换 AI 模型（Claude Sonnet、GPT-5、Gemini 等） |
| `/theme [show\|set\|list] [auto\|dark\|light]` | 配置终端主题 |
| `/terminal-setup` | 启用多行输入模式 |
| `/reset-allowed-tools` | 重置已允许的外部工具权限 |
| `/user [show\|list\|switch]` | 管理 GitHub 账户（个人/企业切换） |

### 外部服务与协作

| 命令 | 功能 |
|------|------|
| `/agent` | 选择自定义 Copilot Agent |
| `/delegate <prompt>` | 创建由 AI 生成的 Pull Request |
| `/share [file\|gist] [path]` | 导出会话记录为 Markdown 文件或 GitHub Gist |
| `/mcp [show\|add\|edit\|delete\|disable\|enable]` | 管理 MCP 服务器配置 |
| `/skills` | 管理增强型能力 |
| `/login` / `/logout` | 登录/登出 Copilot CLI |

### 自然语言 CLI 辅助

| 命令 | 功能 |
|------|------|
| `/explain <command>` | 解释任意 shell 命令的含义 |
| `/suggest <task>` | 根据自然语言任务描述建议 shell 命令 |
| `/revise` | 基于后续指令修改上一次建议 |

### 辅助

| 命令 | 功能 |
|------|------|
| `/help` | 显示所有可用命令 |
| `/feedback` | 向 GitHub 提交产品反馈 |

---

## 三、GitHub.com Copilot Chat（内置 MCP Skills）

在 GitHub.com 网页端使用 Copilot Chat 时，内置了以下 MCP Skill（无需显式使用 `/` 调用，直接用自然语言触发）：

| Skill | 示例用途 |
|-------|----------|
| `create_branch` | 在仓库中创建新分支 |
| `create_or_update_file` | 创建或更新仓库文件 |
| `push_files` | 推送文件到指定分支 |
| `update_pull_request_branch` | 更新 PR 分支到最新 base |
| `merge_pull_request` | 合并 Pull Request |
| `get_me` | 获取当前用户信息 |
| `search_users` | 按名称搜索 GitHub 用户 |

---

## 四、MSSQL 扩展专用命令（VS Code @mssql）

在 VS Code 中使用 `@mssql` chat participant 时的数据库专用命令：

| 命令 | 功能 |
|------|------|
| `/connect` | 打开连接面板建立数据库连接 |
| `/disconnect` | 终止当前连接 |
| `/changeDatabase` | 切换当前服务器上的数据库 |
| `/getConnectionDetails` | 显示当前连接详情 |
| `/runQuery <SQL>` | 在连接数据库上执行 SQL 查询 |
| `/explain <SQL>` | 自然语言解释 SQL 代码 |
| `/fix <SQL>` | 检测并修正 SQL 语法错误 |
| `/optimize <SQL>` | 分析查询性能并提供索引/重写建议 |
| `/showSchema` | 显示数据库架构图（表、关系、键） |
| `/showDefinition <对象>` | 显示表/视图/函数/存储过程的定义 |
| `/listServers` / `/listDatabases` / `/listSchemas` | 列出服务器/数据库/架构 |
| `/listTables` / `/listViews` / `/listFunctions` / `/listProcedures` | 列出表/视图/函数/存储过程 |

---

## 五、与 Claude Code 斜杠命令对比

| 维度 | GitHub Copilot | Claude Code |
|------|---------------|-------------|
| 代码解释 | `/explain` | 直接用自然语言提问 |
| 代码修复 | `/fix` | 直接用自然语言提问 |
| 生成测试 | `/tests` | 直接用自然语言提问 |
| 会话管理 | `/clear` `/compact` `/chronicle` | `/clear` `/compact` `/resume` `/branch` |
| 模型切换 | `/model [model]` | `/model [model]` |
| 计划模式 | 无 | `/plan` |
| 代码审查 | 无内置等价物 | `/code-review` `/security-review` |
| CLI 辅助 | `/explain` `/suggest` `/revise` | `claude -p "query"` |
| Skills | `/skills list/info/reload` `/create-skill` | `/skills`（列出可用 Skill）+ 每个 Skill 可独立调用 |
| MCP | `/mcp` | `/mcp` |

> Claude Code 的哲学是「用自然语言驱动一切」，斜杠命令主要用于**会话/配置/工作流管理**；Copilot 则将**高频代码操作**（explain/fix/tests/doc）也封装为斜杠命令，减少重复提示词的输入。

---

## 参考文献

- [A cheat sheet to slash commands in GitHub Copilot CLI](https://github.blog/ai-and-ml/github-copilot/a-cheat-sheet-to-slash-commands-in-github-copilot-cli/) — GitHub 官方博客
- [GitHub Copilot Chat cheat sheet](https://docs.github.com/en/copilot/reference/chat-cheat-sheet) — GitHub Docs
- [Quickstart: Use GitHub Copilot Slash Commands - MSSQL Extension](https://learn.microsoft.com/en-us/SQL/tools/visual-studio-code-extensions/github-copilot/slash-commands) — Microsoft Learn
- [Introducing Copilot CLI and agentic capabilities enhancements in JetBrains IDEs](https://github.blog/changelog/2026-06-02-introducing-copilot-cli-and-agentic-capabilities-enhancements-in-jetbrains-ides/) — GitHub Changelog 2026-06
- [用 `/create-skill` 打造专属 Copilot 技能](https://pcion123.github.io/2026/04/01/vscode-skill-create/) — 实战教程

## 相关笔记

- [[GitHub Copilot配置体系]] — Copilot 配置体系总览（instructions、agents、skills、hooks）
- [[Claude-Code-Slash-Commands-斜杠命令]] — Claude Code 斜杠命令全集
- [[Skills]] — Skills 概念与两种平台的不同实现
- [[Agent]] — Agent 概念基础
- [[MCP]] — MCP 协议与服务器接入
- [[AI学习路线]] — 返回 AI 学习路线总览
