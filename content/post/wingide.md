---
title: WingIDE教程
date: 2018-07-23 17:12:34
tags:
- WingIDE
- 教程
- 入门
---





# 技巧



## 主动标识变量的类型

[官方教程](https://wingware.com/doc/edit/helping-wing-analyze-code)



```python
from typing import Dict, List

def myFunction(arg1: str, arg2: Dict) -> List:
  return arg2.get(arg1, [])
```



```python
x:int = Something()
```



使用的是[PEP-484](https://www.python.org/dev/peps/pep-0484/)的规范



