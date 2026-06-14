# Trigger Precision 的根源：多 Skill 调度竞争

用户在学习第 1 课（T-P-G 三角）后自行推导出一个关键洞察：多个 skill 可能同时匹配同一个任务，所以需要精确的触发条件来消除歧义。这个理解是正确的。

**Evidence**：用户提问——「在一个 agent 的任务中，多个 skills 能被同时调用吗？我的理解是因为可能有个任务需要多个 skills，才需要写清楚触发条件。」

**Implications**：
- 用户已内化 T-P-G 中的 T（Trigger Precision），并开始思考背后的系统机制
- 下一课应聚焦：skill dispatch 机制 + 多 skill 组合模式（何时拆分、何时合并）
- 后续可深入：workflow 编排多个 skill、skill 间通信
