---
date: 2026-05-20
---
`HEAD指针` 指向当前分支，分支再指向`commit`

>HEAD → main → commit C

`commit`后，分支指针自动前进

> HEAD → main → commit D

## detached HEAD

`detached HEAD` 直接指向某个`commit`

>HEAD → commit C

因此在`detached`的状态下进行`commit`,新的提交不属于任何分支

> [!warning] 在这种情况下切换到另一个分支，该次提交将会难以寻找

## 移动head指针

[[git reset]]

[[git checkout]]