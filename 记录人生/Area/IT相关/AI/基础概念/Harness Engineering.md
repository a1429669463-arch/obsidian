---
date: 2026-05-23
tags:
  - AI
---
[[Agent]] = Model + Harness。  
  
Model 是负责语言理解、推理和生成的核心能力来源。  
  
Harness 是围绕 Model 搭建的一整套运行系统，用来把模型的能力转化为可以完成任务的工作能力。它不仅包括 [[Prompt Engineering]] 和 [[Context Engineering]]，还包括工具调用、状态管理、记忆、文件系统、sandbox、执行流程、权限控制、guardrails、日志、评估和多 Agent 协作等。  
  
Harness Engineering 则是更上层的系统设计问题：如何把模型、prompt、context、tools、memory、workflow、guardrails、evaluation 等组合起来，形成一个稳定、可调试、可验证的 Agent 系统。  

## 关键原则  
  
1. 不要给 Agent 一本大说明书，要给它一张地图。  
2. Agent 看不到的信息，对它来说就不存在。  
3. 文档要版本化，最好放进 repo，成为记录系统。  
4. 规则不要只写在文档里，能机械化就机械化。  
5. 当 Agent 做不好时，不只是重试，而是分析缺少什么：信息、工具、约束还是验证。  
6. 人类不再只是写代码，而是设计环境、标准和反馈回路。
## 参考文献

OpenAI: - [[Harness engineering - leveraging Codex in an agent-first world]]

## 相关笔记

- [[AI学习路线]] — 返回学习路线总览
- [[Agent]] — Model + Harness
- [[Prompt Engineering]] — Harness 的关键组成部分
- [[Context Engineering]] — Harness 的关键组成部分


