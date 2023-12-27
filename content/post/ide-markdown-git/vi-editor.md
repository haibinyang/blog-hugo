---
title: VI经验
date: 2018-07-04 09:28:39
tags:
- VI
- VIM

---



# 操作



| vi command | description                                         |
| ---------- | --------------------------------------------------- |
| 0          | move to beginning of the current line               |
| $          | move to end of line                                 |
| H          | move to the top of the current window (high)        |
| M          | move to the middle of the current window (middle)   |
| L          | move to the bottom line of the current window (low) |
| 1G         | move to the first line of the file                  |
| 20G        | move to the 20th line of the file                   |
| G          | move to the last line of the file                   |



[参考](https://alvinalexander.com/linux/vi-vim-editor-end-of-line)



## 跳转到行的末尾

按`$`键



<kbd>A</kbd>：跳转到行的末尾，并且立即进入插入状态。

I（大写i）：跳转到行的首行，并且立即进入插入状态。



回到首行

```
gg
```





清空

```bash
:1,$d
```





