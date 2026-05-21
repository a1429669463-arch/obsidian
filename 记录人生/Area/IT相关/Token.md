---
tags:
  - AI
date: 2026-05-21
---
`Token`是大语言模型处理文本的基本单位

一个`Token`不相当于一个单词，而是对一段话进行划分后的基本单位

类似于以下的感觉

```
我/今天/买了菜
```

但实际切分方式不一定完全符合自然语言中的词语边界，可能会按字、词、子词、标点符号等方式切分。

`Token`会影响`Ai`的上下文和处理速度

`API`计费时也是按`Token`进行收费(按照输入，指示文长度，输出长度计算`Token`收费)

[token可视化网站](https://platform.openai.com/tokenizer?utm_source=copilot.com)