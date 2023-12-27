---
title: TuShare教程
date: 2018-05-03 14:11:16
tags:
- tushare
---



# 教程



[官网](http://tushare.org/)

[Github](https://github.com/waditu/tushare)

[官方博客（推荐）](https://ask.hellobi.com/blog/waditu/10411#articleHeader7)



# 分钟数据



需要1.0.0版本及以上。

直接在TuShare中获取数据

```python
import tushare as ts

cons = ts.get_apis()

#分钟数据, 设置freq参数，分别为1min/5min/15min/30min/60min，D(日)/W(周)/M(月)/Q(季)/Y(年)
df = ts.bar('000001', conn=cons, freq='1min', start_date='2018-04-09', end_date='2018-04-09')
print(type(df))
print(len(df))
print(df['code', 'close'])
```



在RiceQuant上获取分钟数据来对比

```python
def init(context):
    pass

def after_trading(context):
    logger.info(context.now)
    data = history_bars('000001.XSHE', 240, '1m', ['datetime', 'close'])
    logger.info(len(data))
    logger.info(data)
```







# 历史行情



## get_k_data



[教程链接](https://mp.weixin.qq.com/s?__biz=MzAwOTgzMDk5Ng==&mid=2650833972&idx=1&sn=4de9f9ee81bc8bf85d1e0a4a8f79b0de&chksm=80adb30fb7da3a19817c72ff6f715ee91d6e342eb0402e860e171993bb0293bc4097e2dc4fe9&mpshare=1&scene=1&srcid=1106BPAdPiPCnj6m2Xyt5p2M#wechat_redirect)



```python
import tushare as ts

d = ts.get_k_data('000001', start='2018-04-09', end='2018-04-13')
print(type(d))
print(len(d))
print(d)
```



start和end的时间格式要求

start='2018-04-09' 必须这样写，不能写2018-4-9，这样是错误的。



