# Skills 目录说明

自定义 Skills 放在此目录下，Claude Code 会自动加载。

## 目录结构

```
skills/
├── README.md          ← 本文件
├── 你的skill名/
│   ├── SKILL.md       ← 核心文件（必需），定义 skill 的行为
│   ├── scripts/       ← 可选：可执行脚本
│   ├── references/    ← 可选：参考文档，按需加载到上下文
│   └── assets/        ← 可选：模板、图标等输出用文件
```

## SKILL.md 格式

```markdown
---
name: my-skill-name
description: 这个 skill 做什么、何时触发。描述越具体，触发越准确。
---

# Skill 标题

具体的指令内容...
```

## 三级加载机制

1. **元数据**（name + description）— 始终在上下文中（约 100 词）
2. **SKILL.md 正文** — skill 触发时加载（建议 < 500 行）
3. **捆绑资源**（scripts/references/assets）— 按需加载

## 编写原则

- 用祈使句写指令（"这样做"而非"你应该这样做"）
- 解释**为什么**，而非堆砌 MUST/ALWAYS
- 保持 SKILL.md 精简，大段参考内容放到 references/
- description 是触发机制的核心，写清楚何时使用此 skill

## 参考

- 插件 skills 路径：`~/.claude/plugins/marketplaces/claude-plugins-official/plugins/`
- 用户级 skills：`~/.claude/skills/`
