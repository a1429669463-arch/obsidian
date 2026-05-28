---
date: 2026-05-28
---

常见的 `.xlsx` Excel 文件，本质上是一个 ZIP 压缩包，里面主要由 XML 文件组成

例如把 `.xlsx` 改名成 `.zip` 后解压，通常能看到类似结构：
```
[Content_Types].xml
_rels/
xl/
  workbook.xml
  worksheets/
    sheet1.xml
    sheet2.xml
  sharedStrings.xml
  styles.xml
  drawings/
  media/
```


因此，python去读取 `.xlsx` Excel文件时，本质就是读取xml文件，xml出问题，那么python处理的内容也会出现问题

