# T-P-G 框架的细化：排除条件只是手段之一

用户观察到许多优秀 skill 的 description 中没有显式排除条件，质疑 T2 检查项的必要性。这个观察是正确的。

**修正后的理解**：Trigger Precision 的目标是「该触发时触发，不该触发时不触发」。实现这一目标有 4 种不同机制：窄描述、disable-model-invocation、领域专有词汇、显式排除条件。排除条件只是其中最显式的一种，不是必须的。

**Evidence**：用户自行观察了「名人写的 skills」后提出质疑。同时以 teach SKILL.md 为案例，展示了 `disable-model-invocation: true` 如何从根本上消除了误触发问题。

**Implications**：
- T-P-G 检查清单需要更新：T2 从「是否列出了不触发的情况」改为多选式评估
- 后续课程中教授 T-P-G 时应强调「机制多样，目标唯一」
- 用户已表现出批判性思维——不盲从框架，用实际观察来检验
