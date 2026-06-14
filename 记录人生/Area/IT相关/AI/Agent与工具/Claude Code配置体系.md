---
date: 2026-06-13
tags:
  - AI
  - Claude
  - 配置
status: draft
source: Claude Code VS Code 扩展 2.1.177 + 本地环境文件
---

> 本文档为索引型笔记，概述 Claude Code 的配置体系。详细主题后续拆分为独立笔记。

Claude Code 的配置分为四个层面：**全局设置** → **项目级文件** → **MCP** → **Hooks**。

---

## 一、官方稳定配置

### 全局设置

- **`~/.claude/settings.json`** — 用户级设置文件。
  - `env` — 环境变量（模型名称映射、API 地址等）
  - `effortLevel` — 默认努力级别
  - `includeCoAuthoredBy` — 是否在 git commit 中添加 co-authored-by
- **`~/.claude.json`** — 全局状态文件（迁移标记、用户 ID、首次启动时间等元数据），一般不手动编辑。

### 项目级文件

- **`CLAUDE.md`** — 放在项目根目录，对该项目的 Claude Code 会话生效。相当于 GitHub Copilot 的 `copilot-instructions.md`。
- **`.claude/settings.local.json`** — 项目级设置覆盖，可覆盖全局 `settings.json` 的部分字段。Hooks 通常在此配置。

### MCP 配置

通过在 settings 中配置 MCP 服务器，让 Claude Code 接入外部工具和数据源。详见 [[MCP]]。

### Hooks 系统

在 Agent 生命周期事件（工具调用前后、会话启动/结束等）触发指定的 shell 命令。通常在 `.claude/settings.local.json` 中配置。未来可拆分为独立笔记 [[Claude Code Hooks]]。

---

## 二、当前个人环境

- 通过用户级 `~/.claude/settings.json` 配置第三方 API 代理
- `effortLevel`: max
- `includeCoAuthoredBy`: false
- **未配置**：`CLAUDE.md`、`.claude/settings.local.json`、MCP 服务器、Hooks

---

## 三、社区约定 / 待确认

以下路径在社区讨论中出现，但是否为 Claude Code 官方标准配置**待确认**：

- **`.claude/rules/*.md`** — 规则文件目录，可能作为指令加载（类似 `.github/instructions/`）
- **`.claude/skills/`** — 技能文件目录（类似 GitHub Copilot 的 `SKILL.md` 体系）
- **`.claude/agents/`** — 自定义 Agent 目录

> ⚠️ 以上路径在 Claude Code 官方文档中尚未找到明确说明，使用前请验证。

---

## 四、与 GitHub Copilot 配置对比

| 维度 | Claude Code | GitHub Copilot |
|---|---|---|
| 项目全局指令 | `CLAUDE.md` | `.github/copilot-instructions.md` |
| 文件级指令 | `.claude/rules/`（待确认） | `.github/instructions/` |
| 任务模板 | 无等价物 | `.github/prompts/` |
| 自定义 Agent | `.claude/agents/`（待确认） | `.github/agents/` |
| Skills | `.claude/skills/`（待确认） | `SKILL.md` |
| Hooks | `.claude/settings.local.json` | `.github/hooks/` |
| 全局设置 | `~/.claude/settings.json` | VS Code `settings.json` |
| MCP | 原生支持 | 通过 Agent 扩展支持 |

详见 [[GitHub Copilot配置体系]]。

---

## 相关笔记

- [[AI学习路线]] — 返回学习路线总览
- [[GitHub Copilot配置体系]] — Copilot 配置体系对比
- [[MCP]] — MCP 协议概念
- [[Agent]] — Agent 概念基础
- [[Skills]] — Skills 概念基础
- [[上下文]] — 配置会影响加载到上下文中的内容

## 待创建笔记

- Claude Code Hooks — 生命周期钩子详细配置与实战
- Claude Code MCP 配置实战 — MCP 服务器接入示例
