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



