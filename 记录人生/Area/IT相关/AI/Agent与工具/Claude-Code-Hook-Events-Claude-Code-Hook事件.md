---
date: 2026-06-14
tags:
  - AI
  - Claude Code
  - Hooks
  - 配置
---

Hook 事件（Hook Events）是指 Claude Code 在 Agent 生命周期中预留的一系列挂载点，允许用户在特定时机自动触发自定义 shell 命令、HTTP 请求、MCP 工具或 LLM 评估，从而拦截、修改或观察 AI 的行为。

Claude Code 共定义了 **30 个** Hook 事件，覆盖从会话启动到结束的完整生命周期。

## Hook 类型

每个事件支持不同的 Hook 执行方式（称为 Hook 类型）：

| 类型 | 说明 | 支持异步 |
|------|------|---------|
| `command` | Shell 命令，从 stdin 读取 JSON | ✅ |
| `http` | 向指定 URL 发送 POST 请求 | ❌ |
| `mcp_tool` | 调用已注册的 MCP 工具 | ❌ |
| `prompt` | 单轮 LLM 评估 | ✅ |
| `agent` | 多轮子代理（可调用工具） | ✅ |

> ⚠️ `SessionStart` 和 `Setup` 仅支持 `command` 和 `mcp_tool`。

## 事件总览（按生命周期分组）

### 会话生命周期

| 事件 | 触发时机 | 可拦截 |
|------|---------|:---:|
| **SessionStart** | 会话开始/恢复/`/clear`/压缩后 | ❌ |
| **Setup** | `--init-only` 或 `-p --init` 模式 | ❌ |
| **SessionEnd** | 会话终止 | ❌ |

### 用户输入

| 事件 | 触发时机 | 可拦截 |
|------|---------|:---:|
| **UserPromptSubmit** | 用户提交提示后、AI 处理前 | ✅ |
| **UserPromptExpansion** | 斜杠命令展开为提示前 | ✅ |

### 工具调用（核心高频）

| 事件 | 触发时机 | 可拦截 |
|------|---------|:---:|
| **PreToolUse** | 工具执行前 | ✅ |
| **PermissionRequest** | 权限对话框显示前 | ✅ |
| **PermissionDenied** | 自动模式拒绝了工具调用 | ❌ |
| **PostToolUse** | 工具成功执行后 | ❌ |
| **PostToolUseFailure** | 工具执行失败后 | ❌ |
| **PostToolBatch** | 一批并行工具调用全部完成后 | ✅ |

### 子代理 & 任务

| 事件 | 触发时机 | 可拦截 |
|------|---------|:---:|
| **SubagentStart** | 子代理启动 | ❌ |
| **SubagentStop** | 子代理完成 | ✅ |
| **TaskCreated** | 任务创建 | ✅ |
| **TaskCompleted** | 任务标记完成 | ✅ |

### Agent 循环控制

| 事件 | 触发时机 | 可拦截 |
|------|---------|:---:|
| **Stop** | AI 完成响应时 | ✅ |
| **StopFailure** | 轮次因 API 错误结束 | ❌ |
| **TeammateIdle** | Agent Team 队友即将空闲 | ✅ |

### 消息 & 指令

| 事件 | 触发时机 | 可拦截 |
|------|---------|:---:|
| **MessageDisplay** | 助手消息流式显示时 | ❌ |
| **InstructionsLoaded** | CLAUDE.md / rules 加载到上下文 | ❌ |
| **Notification** | Claude Code 发送通知时 | ❌ |

### 工作区 & 配置

| 事件 | 触发时机 | 可拦截 |
|------|---------|:---:|
| **ConfigChange** | 会话中配置文件变更 | ✅ |
| **CwdChanged** | 工作目录变更 | ❌ |
| **FileChanged** | 监视的文件在磁盘上变更 | ❌ |
| **WorktreeCreate** | 创建 git worktree 时 | ✅ |
| **WorktreeRemove** | 移除 git worktree 时 | ❌ |

### 上下文管理

| 事件 | 触发时机 | 可拦截 |
|------|---------|:---:|
| **PreCompact** | 上下文压缩前 | ✅ |
| **PostCompact** | 上下文压缩完成后 | ❌ |

### MCP 交互

| 事件 | 触发时机 | 可拦截 |
|------|---------|:---:|
| **Elicitation** | MCP 服务器请求用户输入 | ✅ |
| **ElicitationResult** | 用户响应 MCP 征询后 | ✅ |

## 决策控制模式

Hook 通过 **exit code** 或 **JSON 输出** 控制行为：

| 方式 | 效果 |
|------|------|
| `exit 0` | 放行，正常继续 |
| `exit 2` | 阻止当前操作 |
| stdout JSON `{"decision":"block"}` | 阻止并给出原因 |
| stdout JSON `{"hookSpecificOutput":{...}}` | 修改行为（替换参数/注入上下文/设置权限） |

## 关键事件的实战用途

### PreToolUse — 工具调用前拦截

最常用的 Hook 事件，支持 `matcher` 按工具名过滤（如 `Write`、`Bash`、`Edit`）。典型场景：

- 写入前扫描密钥/密码 → 拦截敏感信息泄露
- 危险命令拦截（`rm -rf`、`git push --force`）
- 修改工具输入（如自动补充路径参数）
- 特定文件的写入需额外确认

### PostToolUse — 工具调用后处理

工具执行成功后的副作用：

- 写入后自动格式化（prettier/black）
- 操作审计日志记录
- 自动 git add 已写入文件

### UserPromptSubmit — 提示注入

每次用户发消息时触发（无 matcher，总是触发）：

- 自动注入项目规范到上下文
- 检查用户提示是否包含敏感信息
- 设置会话标题

### Stop — 防止过早停止

AI 完成响应时触发，可阻止停止让 AI 继续，用于质量门禁：

- 检查 AI 是否完成了所有任务
- 验证输出是否满足格式要求

## 配置位置

Hook 在 `.claude/settings.local.json` 中配置：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "python H:/scripts/check-secrets.py",
            "timeout": 5000
          }
        ]
      }
    ]
  }
}
```

## 参考文献

- [Claude Code Hooks 官方文档（中文）](https://code.claude.com/docs/zh-CN/hooks) — 所有事件的详细输入输出格式
- [Claude Code Hooks 官方文档（英文）](https://code.claude.com/docs/en/hooks) — 最新事件列表和决策控制模式

## 相关笔记

- [[Claude Code配置体系]] — Hooks 在 Claude Code 配置体系中的位置
- [[Skills]] — Skill 与 Hook 的分工：Skill 管流程，Hook 管底线
- [[MCP]] — MCP 工具也可以通过 Hook 事件触发
- [[Agent]] — SubagentStart/SubagentStop 与子代理的关系
- [[AI学习路线]] — 返回学习路线总览
