---
date: 2026-06-14
tags:
  - AI
  - Copilot
  - Hooks
  - 配置
---

GitHub Copilot Hook（GitHub Copilot 挂钩）是指在 Copilot Agent 生命周期的特定节点自动触发的外部命令（shell 脚本、HTTP 请求或自动提示），用于实现安全拦截、策略执行、日志审计、结果修改等自定义行为。

Copilot 共定义了 **13 个** Hook 事件，通过 `.github/hooks/*.json` 配置文件管理，兼容 Copilot CLI（本地终端）和 Copilot Cloud Agent（GitHub 云端沙箱）。

## Hook 事件总览

| 事件 | 触发时机 | 可拦截 | 云代理 | matcher |
|------|---------|:---:|:---:|------|
| **sessionStart** | 新会话开始或恢复 | ✅ 注入上下文 | ✅ | — |
| **sessionEnd** | 会话终止 | ❌ | ✅ | — |
| **userPromptSubmitted** | 用户提交提示后 | ❌ | ✅（最多1次） | — |
| **preToolUse** | 工具执行前 | ✅ allow/deny/modify | ✅ | `toolName` |
| **postToolUse** | 工具成功完成后 | ✅ 修改结果 | ✅ | — |
| **postToolUseFailure** | 工具执行失败后 | ✅ 恢复指导 | ✅ | — |
| **agentStop** | 主代理完成一回合 | ✅ block/allow | ✅ | — |
| **subagentStart** | 子代理启动 | ✅ 注入上下文 | ✅ | `agentName` |
| **subagentStop** | 子代理完成 | ✅ block/allow | ✅ | — |
| **errorOccurred** | 执行期间发生错误 | ❌ | ✅ | — |
| **notification** | 系统通知（仅 CLI） | ❌ 注入上下文 | ❌ 不触发 | `notification_type` |
| **permissionRequest** | 权限对话框显示前（仅 CLI） | ✅ allow/deny | ❌ 不触发 | `toolName` |
| **preCompact** | 上下文压缩开始前 | ❌ 仅通知 | ✅（仅 auto） | `trigger` |

## Hook 类型

每个事件可配置以下三种类型的 hook：

| 类型 | 说明 | 适用场景 |
|------|------|---------|
| `command` | 执行 shell 脚本（bash/powershell/command 三选一） | 本地检查、文件操作、日志 |
| `http` | 向指定 URL 发送 JSON POST 请求 | 外部服务集成、云日志 |
| `prompt` | 自动提交文本或斜杠命令 | 注入初始提示（**仅 CLI + sessionStart**） |

## 配置文件格式

Hook 配置文件为 JSON 格式，`version` 字段必填为 `1`。

### 文件位置

| 层级 | 路径 | 云代理可用 |
|------|------|:---:|
| 仓库级 | `.github/hooks/*.json` | ✅ |
| 用户级 | `~/.copilot/hooks/*.json`（Windows: `%USERPROFILE%\.copilot\hooks\`） | ❌ |
| 设置内联 | `.github/copilot/settings.json` 或 `settings.local.json` | ❌ |
| 用户设置 | `~/.copilot/settings.json` | ❌ |
| 插件 | 插件的 `hooks.json` | ❌ |

> ⚠️ **云代理仅加载** `.github/hooks/*.json`（必须位于仓库默认分支）。

### 顶层结构

```json
{
  "version": 1,
  "disableAllHooks": false,
  "hooks": {
    "<事件名>": [ /* hook entry 数组 */ ]
  }
}
```

| 字段 | 类型 | 必填 | 说明 |
|------|------|:--:|------|
| `version` | number | ✅ | 固定为 `1` |
| `disableAllHooks` | boolean | ❌ | `true` 时跳过此文件的所有 hook |
| `hooks` | object | ✅ | 键为事件名，值为 hook entry 数组 |

### Command Hook 字段

默认类型（`type` 可省略），执行本地 shell 脚本：

| 字段 | 类型 | 必填 | 说明 |
|------|------|:--:|------|
| `type` | `"command"` | ❌ | 默认值，可省略 |
| `bash` | string | 三选一 | Unix shell（bash 执行） |
| `powershell` | string | 三选一 | Windows PowerShell |
| `command` | string | 三选一 | 跨平台回退——会复制给 `bash` 和 `powershell`。显式字段优先 |
| `cwd` | string | ❌ | 工作目录（相对仓库根或绝对路径） |
| `env` | object | ❌ | 注入的环境变量（支持 `$VAR` 扩展） |
| `timeoutSec` | number | ❌ | 超时秒数，默认 30 |
| `timeout` | number | ❌ | 同上（`timeoutSec` 同时存在时优先） |

### HTTP Hook 字段

向外部服务发送 JSON POST：

| 字段 | 类型 | 必填 | 说明 |
|------|------|:--:|------|
| `type` | `"http"` | ✅ | — |
| `url` | string | ✅ | 目标 URL，必须为 `https://`（localhost 除外） |
| `headers` | object | ❌ | 自定义请求头 |
| `allowedEnvVars` | string[] | ❌ | 允许在 headers 值中展开的环境变量名 |
| `timeoutSec` | number | ❌ | 默认 30 秒 |

### Prompt Hook 字段

自动提交文本（**仅 CLI + sessionStart**）：

| 字段 | 类型 | 必填 | 说明 |
|------|------|:--:|------|
| `type` | `"prompt"` | ✅ | — |
| `prompt` | string | ✅ | 提交的文本或 `/` 斜杠命令 |

## 完整配置示例

```json
{
  "version": 1,
  "hooks": {
    "sessionStart": [
      {
        "type": "command",
        "bash": "echo \"$(date): session started\" >> /tmp/copilot.log",
        "powershell": "Add-Content -Path $env:TEMP\\copilot.log -Value \"$(Get-Date): session started\"",
        "timeoutSec": 10
      }
    ],
    "preToolUse": [
      {
        "type": "command",
        "matcher": "bash",
        "bash": "./.github/hooks/scripts/security-check.sh",
        "powershell": "./.github/hooks/scripts/security-check.ps1",
        "cwd": ".",
        "timeoutSec": 15
      },
      {
        "type": "http",
        "url": "https://hooks.internal.company.com/copilot-audit",
        "headers": { "X-Team": "platform" },
        "timeoutSec": 10
      }
    ],
    "postToolUse": [
      {
        "type": "command",
        "bash": "cat >> /tmp/copilot-tool-results.jsonl",
        "powershell": "$input | Add-Content -Path $env:TEMP\\copilot-tool-results.jsonl"
      }
    ],
    "agentStop": [
      {
        "type": "command",
        "bash": "./.github/hooks/scripts/quality-gate.sh",
        "powershell": "./.github/hooks/scripts/quality-gate.ps1",
        "timeoutSec": 30
      }
    ]
  }
}
```

## 决策控制输出

Hook 脚本通过 **stdout 单行 JSON** 向 Copilot 返回决策。

### preToolUse（最常用）

```json
// 允许执行
{"permissionDecision": "allow"}

// 拒绝执行（云代理下 ask 视同 deny）
{"permissionDecision": "deny", "permissionDecisionReason": "rm -rf 被安全策略禁止"}

// 修改工具输入后放行
{"permissionDecision": "allow", "modifiedArgs": {"command": "npm run test -- --coverage"}}
```

### postToolUse

```json
// 修改工具结果
{
  "modifiedResult": {
    "resultType": "success",
    "textResultForLlm": "已将日志中的敏感路径替换为占位符..."
  }
}

// 注入额外上下文
{"additionalContext": "注意：该文件的 lint 结果中有 3 个 warning"}
```

### agentStop / subagentStop

```json
// 阻止停止，让 Agent 继续
{"decision": "block", "reason": "请再次检查是否有未提交的 TODO 注释"}

// 允许停止
{"decision": "allow"}
```

## matcher 匹配

matcher 模式锚定完整匹配 `^(?:pattern)$`：

| 事件 | matcher 字段 | 匹配目标示例 |
|------|------------|------|
| `preToolUse` | `toolName` | `bash`, `powershell`, `create`, `edit`, `view`, `glob`, `grep`, `ask_user`, `task`, `web_fetch` |
| `permissionRequest` | `toolName` | 同上 |
| `subagentStart` | `agentName` | 自定义 agent 名 |
| `notification` | `notification_type` | — |
| `preCompact` | `trigger` | `manual`, `auto` |

## CLI vs 云代理 关键差异

| 维度 | CLI（本地） | Cloud Agent（云端） |
|------|-----------|-------------------|
| 操作系统 | 用户机器（Win/Mac/Linux） | Linux 沙箱 |
| Shell | bash + powershell 均可用 | **仅 bash**（powershell 被忽略） |
| 网络 | 完整访问 | 仅 GitHub/Copilot 主机名 + 管理员白名单 |
| 交互性 | 完整（`ask` 弹对话框） | **非交互**（工具调用预批准，无用户对话框） |
| 配置来源 | 多来源合并（仓库/用户/插件/内联） | **仅** `.github/hooks/*.json` |
| 文件持久 | 文件持久保存 | **临时**（hook 写入的文件在任务结束后丢弃） |
| 环境变量 | 继承用户环境 | 仅 `GITHUB_COPILOT_API_TOKEN`、`GITHUB_COPILOT_GIT_TOKEN`、`COPILOT_AGENT_PROMPT` |

## preToolUse 故障行为（重要）

- **Command hook**：**故障关闭**（fail-closed）——脚本崩溃或非零退出码 → 拒绝工具调用
- **HTTP hook**：**故障打开**（fail-open）——网络错误或非 2xx 响应 → 进入默认权限流

## 参考文献

- [GitHub Copilot Hooks 参考（中文）](https://docs.github.com/zh/copilot/reference/hooks-reference) — 事件、配置字段、输入输出的完整规范
- [GitHub Copilot Hooks 参考（英文）](https://docs.github.com/en/copilot/reference/hooks-reference) — 最新英文版
- [使用 Hook 自定义代理工作流](https://docs.github.com/zh/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/use-hooks) — 云代理 Hook 实战教程

## 相关笔记

- [[GitHub Copilot配置体系]] — Hook 在 Copilot 配置体系中的位置
- [[Claude-Code-Hook-Events-Claude-Code-Hook事件]] — Claude Code Hook 事件对比
- [[Skills]] — Skill 与 Hook 的分工：Skill 管流程，Hook 管底线
- [[AI学习路线]] — 返回学习路线总览
