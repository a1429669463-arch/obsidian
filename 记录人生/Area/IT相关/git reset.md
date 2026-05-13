---
date: 2026-05-13
tags:
  - IT
  - git
---
主要作用是移动当前head指针
分为3种模式
- soft:移动 HEAD,保留暂存区和工作区
- mixed:移动 HEAD和暂存区的add,保留工作区
- hard:移动 HEAD,删除暂存区和工作区的改修

```
git reset 模式 位置(Head~1,commit id)
```

> [!note]- 笔记
> `HEAD~n` 表示从当前提交往前数第 n 个提交。
>
> 例如：
> - `HEAD~1`：当前提交的上一个提交
> - `HEAD~2`：当前提交往前两个提交
> - `HEAD~3`：当前提交往前三个提交



