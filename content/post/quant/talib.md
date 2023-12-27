---
title: TA-Lib教程
date: 2018-05-03 11:56:49
tags:
- talib
- 教程
---



# 简介



[官网](http://ta-lib.org/)

文档

- [英文](https://mrjbq7.github.io/ta-lib/)
- [中文](https://github.com/HuaRongSAO/talib-document)



[Python wrapper for TA-Lib](http://mrjbq7.github.io/ta-lib/func.html)





# 入门



第1个

```python
import numpy as np
import talib

close = np.random.random(5)
print(type(close))
print(len(close))
print(close)

output = talib.SMA(close, timeperiod=5)
print(type(close))
print(len(output))
print(output)
```



第2个

```python
import numpy as np
from talib import abstract
# import talib

sma = abstract.SMA
# sma = abstract.Function('sma')

# note that all ndarrays must be the same length!
inputs = {
    'open': np.random.random(10),
    'high': np.random.random(10),
    'low': np.random.random(10),
    'close': np.random.random(10),
    'volume': np.random.random(10)
}

output = sma(inputs, timeperiod=5, price='open')
output2 = sma(inputs, timeperiod=5, price='close')
print(type(output))
print(output)
print(type(output2))
print(output2)
```





