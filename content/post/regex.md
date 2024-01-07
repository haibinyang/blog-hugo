---
title: 正则表示式
date: 2018-05-15 15:36:14
tags:
- regex
- 正则表示式
---



# 工具

https://regex101.com/

非常好的一个学习正则表示式的工具



# 开头和结尾

开头：^

结尾：$



# 转义

.是特殊字符，需要转义



# \b表示单词的结尾

正则公式：

\babc\b

测试的字符串：

peter abc KKabc

KKabc中的abc匹配不到



# 可选单词列表



```basic
(?:XSHE|XSHG|INDX)
```



# Negative Lookahead



`(?!...)`：这是一个负向前瞻断言（negative lookahead assertion），用于指定一个不期望出现的模式。

例如

```bash
(?!api|favicon.ico)
```

例如

```
/((?!api|_next/static|_next/image|favicon.ico).*)
```

- `/`: 正则表达式的开始和结束标志
- `.*`：这表示匹配任何字符（`.`）出现任意次数（`*`），也就是说，匹配除了特定模式外的任何字符串。



# 转义

`\\.` 用于转义点`.`（因为点在正则表达式中意味着任何字符）

例如：

**`.\*\\..\*`** - 这个模式用于避免匹配看起来具有文件扩展名的 URL（例如，`.js`、`.css`、`.html`）。
