---
title: DataTable GroupBy方法
date: 2015-7-15 9:16:19
categories: .Net
toc: true
---

# DataTable GroupBy()
``` csharp
DataTable.AsEnumerable().GroupBy()
```
中间加上`.AsEnumerable()`方法转化为集合
实际使用方式大致如下:

dt.AsEnumerable().GroupBy([LINQ表达式](/2016/01/26/LINQ/))

实际上Group By和Select都属于LINQ的使用范畴。
