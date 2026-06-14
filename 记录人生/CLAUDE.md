# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 这是什么仓库

这是一个 **Obsidian 个人知识库**（vault），通过 Git 做版本管理。内容以中文为主，用 [[wikilink]] 在笔记间建立连接。

## 目录结构

- `HOME.md` — 总 MOC，人类和 AI 共享的目录索引
- `00收件箱/` — 新笔记默认入口，定期归类到其他目录
- `01日记/` — 每日日记（Obsidian Daily Notes 插件自动创建）
- `Area/` — 持续积累的知识领域（IT 技术、Excel 等）
- `Projects/` — 有明确终点的项目（FWL、异度之刃攻略等）
- `Archive/` — 已完成项目归档
- `.obsidian/` — Obsidian 插件和设置，不要手动编辑
- `.claude/skills/` — 本仓库的自定义 Claude Code Skills
- `copilot/` — GitHub Copilot 自定义 prompts 和会话存档

## 笔记规范

创建或编辑笔记时必须遵守（详见 `HOME.md` 底部和 `.claude/skills/study-note/references/note-rules.md`）：

1. **文件命名**：`<英文>-<中文>.md`，如 `LLM-大语言模型.md`
2. **frontmatter 必填**：`date`（YYYY-MM-DD）和 `tags`（至少 1 个）
3. **每篇必有**：`## 参考文献`（至少 1 个 URL）+ `## 相关笔记`（至少 1 个 [[wikilink]]）
4. **开头一句话定义**：`<概念名>（<英文名>）是指 <一句话定义>。`
5. **新笔记先进** `00收件箱/`，除非用户明确指定位置
6. **不得自动修改 MOC**（HOME.md、AI学习路线.md 等），除非用户明确要求
7. **原子化**：一个文件一个概念，子概念只在「可被独立引用 + 有独立结构 + 需要独立参考文献」时才拆分
8. **不确定信息标注**：来源无法验证 → `<!-- 待确认 -->`；多种说法矛盾 → 增「⚠️ 存在争议」节
9. **安全**：密码、密钥、Token 不得以纯文本写入

## 自定义 Skills

本仓库有 2 个自定义 Skill，由 `.claude/skills/<name>/SKILL.md` 定义：

| Skill | 用途 | 触发条件 |
|-------|------|---------|
| `study-note` | 网络搜索 → 创建符合规范的原子笔记 | 用户表达学习意图且期望产出笔记 |
| `feynman-test` | 基于已有笔记出题，费曼学习法检验理解 | 用户要求「考考我」「检验理解」「费曼测试」 |

Skill 的参考文件（references/、templates/）在 Skill 触发后按需加载，不要预读。

## 常用操作

### 创建学习笔记
用 `/study-note` 或直接说「帮我学习 XX」「查询 XX 并写成笔记」。Skill 会自动搜索已有笔记、网络查资料、规划文件、按规范写入。

### 搜索知识库
- 按关键词搜笔记：`Grep "关键词" --glob "**/*.md"` 或直接 `Grep "关键词"` 在当前目录
- 找特定目录下的笔记：`Glob "Area/IT相关/AI/**/*.md"`

### Git
- 所有笔记变更通过 Git 管理
- 用户 commit 风格：中文消息、按日期提交、merge 合并
- 不要 amend 或 force push

## 权限配置

`.claude/settings.local.json` 中配置了 allowlist：`WebSearch` 和 `WebFetch(domain:github.com)`。访问其他域名的 WebFetch 或运行 Bash 命令需要用户确认。
