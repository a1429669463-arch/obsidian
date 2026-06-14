---
date: 2026-06-13
tags:
  - AI
  - Copilot
  - 配置
status: draft
source: VS Code Copilot 扩展 0.52.0 内置文档
---

> 本文档为索引型笔记，概述 GitHub Copilot（VS Code 扩展）的配置体系。详细主题后续拆分为独立笔记。

GitHub Copilot 的配置分为三个层面：**基础指令配置**（控制 AI 行为）、**Agentic 扩展**（赋予 Agent 能力）、**VS Code 设置**（编辑器级开关）。

---

## 一、基础指令配置（官方稳定）

### 项目全局指令

- **`.github/copilot-instructions.md`** — 放在仓库根目录，对该仓库中所有 Copilot 会话生效。
- **`AGENTS.md`** — 与 `copilot-instructions.md` 功能等价，二者选其一即可。

### 文件级指令

- **`.github/instructions/*.instructions.md`** — 通过 YAML frontmatter 中的 `applyTo` 字段指定该指令适用于哪些文件（支持 glob 模式匹配），实现「不同文件不同规则」。

### 任务模板

- **`.github/prompts/*.prompt.md`** — 可复用的任务提示模板。frontmatter 可指定适用的 `agent`、`model`、`tools` 等。

---

## 二、Agentic 扩展（官方支持）

这些文件赋予 Copilot 超越「问答」的自主行为能力：

- **`.github/agents/*.agent.md`** — 自定义 Agent 角色，可指定专属工具、指令、模型回退策略。
- **`SKILL.md`** — 按需加载的技能文件（放在 `.github/skills/<name>/` 下）。AI 先读取元信息判断是否调用，调用后将指令层加载到 [[上下文]]。
- **`.github/hooks/*.json`** — 生命周期钩子，在 Agent 事件（如工具调用前后）触发 shell 命令。

---

## 三、VS Code 设置

在 `settings.json` 中通过 `github.copilot.chat.*` 配置。

**稳定项**（均支持 `file` 指向外部 .md 或 `text` 内联字符串）：

| 设置项 | 作用 |
|---|---|
| `codeGeneration` | 代码生成指令 |
| `testGeneration` | 测试生成指令 |
| `commitMessageGeneration` | 提交信息生成指令 |
| `pullRequestDescriptionGeneration` | PR 描述生成指令 |
| `advanced.review` | 代码审查指令 |

**高级 / 待确认项**（以下设置项来源为扩展 schema，是否作为稳定公开 API 待确认）：

- `customInstructionsInSystemMessage` — 自定义指令在 system message 中的位置
- `omitBaseAgentInstructions` — 是否跳过默认 Agent 指令
- `organizationInstructions` — 组织级指令
- `backgroundAgent` — 后台 Agent 开关

---

## 四、指令发现机制（待确认）

Copilot 从多个来源收集指令。可能的来源类型包括：`home`、`repo`、`model`、`vscode`、`nested-agents`、`child-instructions`、`plugin`。

> ⚠️ 具体合并顺序和优先级待官方文档确认，以上仅为来源枚举。

设置 `enableOnDemandInstructionDiscovery` 开启后，Copilot 会在检测到 `AGENTS.md` / `CLAUDE.md` / `copilot-instructions.md` 时按需加载指令。

---

## 五、当前个人环境

- `.github/` 目录：未创建
- `.github/copilot-instructions.md`：未配置
- VS Code `github.copilot.chat.*` 设置：均未配置
- Copilot 自定义指令功能：当前未使用

---

## 相关笔记

- [[AI学习路线]] — 返回学习路线总览
- [[Claude Code配置体系]] — Claude Code 配置体系对比
- [[Skills]] — SKILL.md 的概念基础
- [[Prompt Engineering]] — 指令设计方法论
- [[渐进式披露]] — 与指令按需加载的关系
- [[Context Engineering]] — 上下文设计的更上层视角

## 待创建笔记

- GitHub Copilot Prompt Files — `.prompt.md` 的详细写法与最佳实践
- GitHub Copilot Agents — `.agent.md` 自定义 Agent 详细配置
- GitHub Copilot Hooks — 生命周期钩子实战
