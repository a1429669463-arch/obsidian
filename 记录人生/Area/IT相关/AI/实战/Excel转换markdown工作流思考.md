---
date: 2026-05-23
---
# Excel 转 Markdown 工作流

## 背景

之前每次让 AI 将 Excel 转换为 Markdown 时，AI 都会重新编写 Python 脚本，导致大量时间花在脚本生成和修正上，稳定性不足。

## 改进思路

将“每次从零生成脚本”改为“基于 pattern 复用成功脚本”。

## 工作流

1. 维护共通指示文 prompt.md  
   记录 Excel 转 Markdown 的共通规则，例如：
   - 所有 sheet 输出到同一个 Markdown
   - 有图片的 sheet 需要标注
   - 删除线需要保留
   - SQL 使用 code block 包裹

2. 维护 pattern.md / pattern_xxx.md  
   记录每种 Excel pattern 的特点，包括：
   - 判断条件
   - 表格结构
   - 说明文字位置
   - SQL 位置
   - 常见转换问题
   - 验收标准

3. 维护 pattern 对应的成功脚本  
   每种 pattern 保存一个曾经成功转换过的 Python 脚本。

4. 让 AI 基于 pattern 选择脚本  
   AI 不从零写脚本，而是参考对应 pattern 的成功脚本做最小修改。

5. 生成 raw.md  
   由 Python 脚本机械转换，尽量保留原始信息。

6. AI 整形生成 final.md  
   根据统一格式要求，将 raw.md 整理成更可读的 Markdown。

7. 人工 review  
   检查是否存在：
   - 内容遗漏
   - 表格错位
   - SQL 未正确包裹
   - 删除线丢失
   - 图片 sheet 未标注

8. 成功后沉淀经验  
   如果这次转换成功，将修改后的脚本、问题和规则补充回对应 pattern。