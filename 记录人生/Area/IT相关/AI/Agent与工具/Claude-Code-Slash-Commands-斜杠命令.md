---
date: 2026-06-14
tags:
  - Claude
  - Claude Code
  - 斜杠命令
---

Claude Code 斜杠命令（Slash Commands）是指在 Claude Code 交互式 REPL 中以 `/` 开头的指令，用于会话管理、模型设置、配置调整、代码审查、插件管理等操作。键入 `/` 即可在交互界面查看实时命令列表和模糊搜索。

---
/
## 会话管理

| 命令 | 功能 |
|------|------|
| `/help` | 显示帮助 |
| `/clear` | 清空对话（别名 `/reset`、`/new`） |
| `/compact [instructions]` | 压缩对话上下文以节省 token，可附带聚焦指令 |
| `/rewind` | 回退对话和/或代码（别名 `/checkpoint`） |
| `/resume [session]` | 恢复之前的会话（别名 `/continue`） |
| `/rename [name]` | 重命名当前会话 |
| `/branch [name]` | 将对话分支到新会话（别名 `/fork`） |
| `/export [filename]` | 导出对话到文件或剪贴板 |
| `/exit` | 退出 REPL（别名 `/quit`） |

## 模型与行为

| 命令 | 功能 |
|------|------|
| `/model [model]` | 交互式选择模型，保存为默认 |
| `/effort [low\|medium\|high\|xhigh\|max\|auto]` | 设置推理努力级别；`max` 需 Opus 4.6+ |
| `/fast [on\|off]` | 切换快速模式（使用 Opus 更快输出） |
| `/sandbox` | 切换沙盒模式 |
| `/plan [description]` | 进入计划模式，先在方案层面获得用户确认再编码 |
| `/focus` | 切换专注视图 |

## 上下文与用量

| 命令 | 功能 |
|------|------|
| `/context` | 用彩色网格可视化上下文窗口占用 |
| `/cost` | 查看 token 用量和费用 |
| `/usage` | 用量仪表板（计划额度、速率限制、费用、统计） |
| `/usage-credits` | 配置额外用量以应对速率限制 |
| `/insights` | 生成会话分析报告 |

## 项目与配置

| 命令 | 功能 |
|------|------|
| `/init` | 初始化项目 `CLAUDE.md` |
| `/config` | 打开设置界面（别名 `/settings`） |
| `/theme` | 主题选择器 |
| `/color [color\|default]` | 设置提示栏颜色 |
| `/statusline` | 配置状态栏 |
| `/keybindings` | 打开快捷键配置 |
| `/terminal-setup` | 配置终端快捷键 |
| `/doctor` | 诊断安装健康状态 |
| `/add-dir <path>` | 添加额外工作目录 |
| `/permissions` | 查看/更新权限（别名 `/allowed-tools`） |
| `/hooks` | 查看 Hook 配置 |
| `/reload-plugins` | 重新加载插件 |
| `/reload-skills` | 重新扫描 Skills 目录 |

## 插件、Skills 与 MCP

| 命令 | 功能 |
|------|------|
| `/plugin` | 管理插件 |
| `/skills` | 列出可用 Skills |
| `/agents` | 管理 Agent 配置 |
| `/mcp` | 管理 MCP 服务器和 OAuth |
| `/memory` | 编辑 CLAUDE.md，切换自动记忆 |

## Git 与代码审查

| 命令 | 功能 |
|------|------|
| `/diff` | 交互式查看未提交更改的 diff |
| `/code-review [effort]` | 审查 diff 中的正确性 bug（内置 Skill） |
| `/simplify` | 代码质量清理审查（复用/简化/效率） |
| `/security-review` | 分析分支安全漏洞 |
| `/ultrareview` | 云端多 Agent 代码审查 |
| `/copy [N]` | 复制 assistant 回复到剪贴板 |

## 工作流与自动化

| 命令 | 功能 |
|------|------|
| `/tasks` | 列出/管理后台任务 |
| `/schedule [description]` | 创建/管理云端定时任务 |
| `/loop [interval] <prompt>` | 按固定间隔重复运行提示词 |
| `/batch <instruction>` | 使用 worktree 编排大规模并行修改 |
| `/workflows` | 查看运行中和已完成的动态工作流 |

## 内置 Skills（以 / 调用）

以下 Skills 内置在 Claude Code 中，可直接用 `/` 调用：

| 命令 | 功能 |
|------|------|
| `/deep-research` | 深度研究——多源搜索、对抗验证、合成带引用的研究报告 |
| `/claude-api` | 加载 Claude API 参考，辅助编写 API 调用代码 |
| `/update-config` | 通过 settings.json 配置 Hooks、权限、环境变量等 |
| `/keybindings-help` | 自定义键盘快捷键 |
| `/verify` | 验证代码变更是否按预期工作（运行应用观察行为） |
| `/init` | 初始化项目 `CLAUDE.md` |
| `/run` | 启动并驱动项目应用，确认变更生效 |
| `/review` | 审查 Pull Request |
| `/fewer-permission-prompts` | 扫描历史记录，生成权限 allowlist 以减少提示 |

> **注意**：`/code-review`、`/simplify`、`/security-review`、`/loop`、`/batch` 也是内置 Skill，已在上方对应分类中列出。插件安装的 Skill 使用 `/plugin-name:skill-name` 格式调用。

## 集成

| 命令 | 功能 |
|------|------|
| `/ide` | 管理 IDE 集成 |
| `/chrome` | 配置 Chrome 浏览器集成 |
| `/desktop` | 继续在桌面应用中处理（别名 `/app`） |
| `/mobile` | 生成移动端扫码二维码（别名 `/ios`、`/android`） |
| `/remote-control` | 从 claude.ai 远程控制（别名 `/rc`） |
| `/remote-env` | 配置默认远程环境 |
| `/install-github-app` | 配置 GitHub Actions App |
| `/install-slack-app` | 安装 Slack App |

## 账号与反馈

| 命令 | 功能 |
|------|------|
| `/login` | 切换 Anthropic 账号 |
| `/logout` | 退出当前账号 |
| `/status` | 显示版本、模型、账号信息 |
| `/feedback` | 提交反馈（别名 `/bug`） |
| `/release-notes` | 查看更新日志 |
| `/privacy-settings` | 隐私设置（Pro/Max 专属） |
| `/upgrade` | 打开升级页面 |
| `/passes` | 分享一周免费 Claude Code 使用权 |

## 已弃用/更名命令

| 命令 | 状态 |
|------|------|
| `/review` | 已弃用，替代为 `/code-review` |
| `/output-style` | 自 v2.1.73 起弃用 |
| `/fork` | 重命名为 `/branch`（别名仍可用） |
| `/pr-comments` | 自 v2.1.91 起移除 |
| `/vim` | 自 v2.1.92 起移除，改用 `/config → Editor mode` |

## CLI 命令（非交互式）

除交互式 `/` 命令外，Claude Code 还支持终端 CLI 命令：

| CLI 命令 | 用途 |
|----------|------|
| `claude` | 启动交互式会话 |
| `claude -p "query"` | 单次查询后退出（非交互模式） |
| `claude -c` | 继续最近一次对话 |
| `claude -r "<session>"` | 按 ID 或名称恢复会话 |
| `claude update` | 更新到最新版本 |
| `claude mcp` | 配置 MCP 服务器 |
| `claude plugin` | 管理插件 |
| `claude auth login/logout/status` | 认证管理 |

常用 CLI 参数：`--model`（指定模型）、`--verbose`（详细日志）、`--output-format json`（JSON 输出）、`--max-turns`（限制轮次）、`--add-dir`（添加工作目录）、`--worktree`（隔离 git worktree）。

---

## 参考文献

- [CLI reference - Claude Code Docs](https://code.claude.com/docs/en/cli-reference)
- [Claude Code slash commands - claude-howto](https://github.com/luongnv89/claude-howto/blob/main/zh/01-slash-commands/README.md)
- [Awesome Claude Code Plugins](https://github.com/ccplugins/awesome-claude-code-plugins) — 社区插件、命令与配置合集
- [wshobson/commands](https://github.com/wshobson/commands) — 57+ 生产级自定义命令

## 相关笔记

- [[Claude Code配置体系]] — 配置体系总览（全局设置、项目文件、MCP、Hooks）
- [[Skills]] — Skills 概念与自定义 Skill 编写
- [[Agent]] — Agent 概念基础
- [[MCP]] — MCP 协议与服务器接入
- [[AI学习路线]] — 返回 AI 学习路线总览
