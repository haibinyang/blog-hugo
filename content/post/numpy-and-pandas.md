---
title: Numpy和Pandas
date: 2018-04-28 11:30:01
tags:
- numpy
- pandas
- 教程
---



# 教程



- [在线](https://morvanzhou.github.io/tutorials/data-manipulation/np-pd/)
- 《量化投资-以Python为工具》
- 《利用Python进行数据分析》

# 数据结构



| 类型   |           | 维度     | 说明                                                         |
| ------ | --------- | -------- | ------------------------------------------------------------ |
| Python | list      | 一维     |                                                              |
| Numpy  | ndarray   | 多维     |                                                              |
| Pandas | Series    | 有序字典 | index：index； values: ndarray                               |
|        | DataFrame | 二维     | index（索引）：index  values: ndarray（一维） values: ndarray（一维） 相比Series共享一个index |



# Python



## range()

不包含末尾

```python
r1 =range(2, 5)
print(type(r1)) # <class 'range'>
print(r1) # range(2, 5)

for i in r1:
    print(i)
    
# 2
# 3
# 4
```





## 创建['A', 'B', 'C', 'D']的样式

list()

创建['A', 'B', 'C', 'D']的快捷方式

```python
list('ABCD') # ['A', 'B', 'C', 'D']
```



# NumPy





## 基础结构

```python
import numpy as np

# 列表转化为矩阵
ndarray = np.array([[1, 2, 3],
                    [2, 3, 4]]
                   )

print(type(ndarray))  # <class 'numpy.ndarray'>

print(repr(ndarray))
# array([[1, 2, 3],
#        [2, 3, 4]])

print(ndarray)
# [[1 2 3]
#  [2 3 4]]

print('ndim = {}'.format(ndarray.ndim))
print('shape = {}'.format(ndarray.shape))
print('size = {}'.format(ndarray.size))
print('dtype = {}'.format(ndarray.dtype))

# ndim = 2
# shape = (2, 3)
# size = 6
# dtype = int64

```



## 创建

**从列表**



```python
n2 = np.array([0, 1, 2, 3])
```



```python
np.array([
    [1, 2, 3],
    [4, 5, 6]
])
```



**range()**

```python
n2 = np.array(range(5)) # 相当于range(0, 5), 不含结束值
print('ndim = {}'.format(n2.ndim)) # ndim = 1
print(type(n2))
print(n2.shape) # (5,)
print(n2) # [0 1 2 3 4]
```



**np.arange()**

```python
n3 = np.arange(2, 8, 2) # 不含结束值
print(n3.ndim) # 1
print(n3.shape) # (3,)
print(n3) # [2 4 6]
```



**np.linspace()**

```python
n4 = np.linspace(2, 8, 4) # 最后一个参数是指定个数
print(n4.shape)
print(n4.dtype) # float64
print(n4) # [ 2.  4.  6.  8.]

n5 = np.linspace(2, 8, 4, dtype=int) # 最后一个参数是指定个数
print(n5.shape)
print(n5.dtype) # int64
print(n5) # [2 4 6 8]
```



**np.random.randn()**

```python
n2 = np.random.randn(5)
print(type(n2)) # <class 'numpy.ndarray'>
print(n2)
```



**其它**

- `zeros`：创建数据全为0
- `ones`：创建数据全为1
- `empty`：创建数据接近0
- `arrange`：按指定范围创建数据
- `linspace`：创建线段



## 显示



为了保持显示的格式的一致性，如果成员中有太大或太少的数字，会转成科学计算的方式（scientific notation）。



```python
import pandas as pd
import numpy as np

df = pd.DataFrame(
        {'open': [130025600.0, 117312800.0], 'close': [3.3, 5.5], 'low': [20180413000000, 20180413000000]}, index=pd.to_datetime(['2018-05-1', '2018-06-01']))
print(df)
#             close             low         open
# 2018-05-01    3.3  20180413000000  130025600.0
# 2018-06-01    5.5  20180413000000  117312800.0
print(df.dtypes)
# close    float64
# low        int64
# open     float64
# dtype: object

np.set_printoptions(formatter={'float':"{:10.16g}".format})
f = df.as_matrix()
print(type(f)) # <class 'numpy.ndarray'>
print(f.dtype) # float64
print(f)
# [[       3.3 20180413000000  130025600]
#  [       5.5 20180413000000  117312800]]

# 恢复
np.set_printoptions()
print(f)
# [[  3.30000000e+00   2.01804130e+13   1.30025600e+08]
#  [  5.50000000e+00   2.01804130e+13   1.17312800e+08]]
```



[参考1](https://stackoverflow.com/questions/30024342/converting-dataframe-to-numpy-array-causes-all-numbers-to-be-printed-in-scientif?rq=1)

[参考2](https://codeday.me/bug/20170323/8682.html)

[参考3](https://docs.python.org/2/library/string.html#format-specification-mini-language)

[参考4](https://tutel.me/c/programming/questions/30024342/converting+dataframe+to+numpy+array+causes+all+numbers+to+be+printed+in+scientific+notation)



## 数据类型



转成原生的Native类型

```python
import numpy as np

# examples using a.item()
type(np.float32(0).item()) # <type 'float'>
type(np.float64(0).item()) # <type 'float'>
type(np.uint32(0).item())  # <type 'long'>

# examples using np.asscalar(a)
type(np.asscalar(np.int16(0)))   # <type 'int'>
type(np.asscalar(np.cfloat(0)))  # <type 'complex'>
type(np.asscalar(np.datetime64(0, 'D')))  # <type 'datetime.datetime'>
type(np.asscalar(np.timedelta64(0, 'D'))) # <type 'datetime.timedelta'>
```



[参考](https://stackoverflow.com/questions/9452775/converting-numpy-dtypes-to-native-python-types)



## 二维



创建

```python
n = np.array([[1, 2, 3], [2, 3,4]])
print(n.ndim) # 2
print(n.shape) (2, 3)
print(n)
```



合并

```python
import numpy as np
A = np.array([1,1,1])
B = np.array([2,2,2])
         
print(np.vstack((A,B)))    # vertical stack
"""
[[1,1,1]
 [2,2,2]]
"""
```

[参考](https://morvanzhou.github.io/tutorials/data-manipulation/np-pd/2-6-np-concat/)





# Pandas



## Series



### 结构



```python
import pandas as pd

s2 = pd.Series([1, 3, 5, 7, 9], index=['a', 'b', 'c', 'd', 'e'])

print(type(s2)) # <class 'pandas.core.series.Series'>
print(s2)
# a    1
# b    3
# c    5
# d    7
# e    9
# dtype: int64

print(s2.index) # Index(['a', 'b', 'c', 'd', 'e'], dtype='object')
print(s2.values) # [1 3 5 7 9]
print(type(s2.index)) # <class 'pandas.core.indexes.base.Index'>
print(type(s2.values)) # <class 'numpy.ndarray'>
```





**指定index和values**

```python
pd.Series([1, 3, 5, 7, 9], index=['a', 'b', 'c', 'd', 'e'])
```



**从dict**

```python
pd.Series({'k1':'v1', 'k2':'v2'})
```



仅指定vales

- 从列表

- 使用np.arrange()

- 使用np.random.randn()

  > 自动生成index

```python
pd.Series([3, 5, 6, 7])
pd.Series(np.arange(2, 6))
pd.Series(np.random.randn(5))
```



### Time Series

Python中的datetime转成Pandas中的**Timestamp**，然后当成Index输入

```python
date = datetime(2018, 4, 27)
print(type(date)) # <class 'datetime.datetime'>
print(date)

ts = pd.Timestamp(date)
print(type(ts)) # <class 'pandas._libs.tslib.Timestamp'>
print(ts)

s1 = pd.Series(2, index=[ts])
print(type(s1.index)) 
# <class 'pandas.core.indexes.datetimes.DatetimeIndex'>
print(s1.index) 
# DatetimeIndex(['2018-04-27'], dtype='datetime64[ns]', freq=None)
print(type(s1)) 
# <class 'pandas.core.series.Series'>
print(s1)
```



使用pd.to_datetime()将字符串列表批量转成DatetimeIndex

```python
pd.Series([5, 6], index=pd.to_datetime(['2018-05-1', '2018-06-01']))
```



细节

```python
date_str = ['2018-04-27', '2018-04-28']
date_index = pd.to_datetime(date_str)

print(type(date_index)) 
# <class 'pandas.core.indexes.datetimes.DatetimeIndex'>
print(date_index) 
# DatetimeIndex(['2018-04-27', '2018-04-28'], dtype='datetime64[ns]', freq=None)

s2 = pd.Series([1, 3], index=date_index)
print(s2)
```



Pandas自动将datetime转换成Timestamp对象

```python
pd.Series([3, 5], index=[datetime(2018, 4, 27), datetime(2018, 5, 17)])
```



批量产生连续的DatetimeIndex

```python
dates = pd.date_range('2013-01-01', periods=6)
print(type(dates)) # <class 'pandas.core.indexes.datetimes.DatetimeIndex'>
print(dates)
```



### 选择数据

[参考](https://morvanzhou.github.io/tutorials/data-manipulation/np-pd/3-2-pd-indexing/)



```python
import numpy as np
import pandas as pd

array = np.arange(24).reshape(6, 4)
dates = pd.date_range('2013-01-01', periods=6)
df = pd.DataFrame(array, columns=list('ABCD'), index=dates)

print(df)


"""
             A   B   C   D
2013-01-01   0   1   2   3
2013-01-02   4   5   6   7
2013-01-03   8   9  10  11
2013-01-04  12  13  14  15
2013-01-05  16  17  18  19
2013-01-06  20  21  22  23
"""
```



**根据index选择row**

一行

```python
d3 = df.loc['20130102']

print(type(d3))
# <class 'pandas.core.series.Series'>

print(d3)
# A    4
# B    5
# C    6
# D    7
# Name: 2013-01-02 00:00:00, dtype: int64
```

多行

```python
d5 = df['20130102':'20130104']

print(type(d5))
# <class 'pandas.core.frame.DataFrame'>

print(d5)
#              A   B   C   D
# 2013-01-02   4   5   6   7
# 2013-01-03   8   9  10  11
# 2013-01-04  12  13  14  15
```



```python
d6 = df[0:3]

print(type(d6))
print(d6)
#             A  B   C   D
# 2013-01-01  0  1   2   3
# 2013-01-02  4  5   6   7
# 2013-01-03  8  9  10  11
```



**选择column**

一列

```python
d2 = df['A']
d2 = df.A

print(type(d2)) 
# <class 'pandas.core.series.Series'>

print(d2)
# 2013-01-01     0
# ...
# 2013-01-06    20
# Freq: D, Name: A, dtype: int64
```



多列

```python
d2 = df[['A', 'B', 'C']]
```





### Index



### get_loc()



```python
import pandas as pd

unique_index = pd.Index(list('abc'))
print(type(unique_index)) # <class 'pandas.core.indexes.base.Index'>
print(unique_index) # Index(['a', 'b', 'c'], dtype='object')

c = unique_index.get_loc('b')
print(type(c)) # <class 'int'>
print(c) # 1
```



**转成list**

```python
series.tolist()
```



## DataFrame



### 创建



```python
import numpy as np
import pandas as pd


dates = pd.date_range('20160101', periods=3)
df = pd.DataFrame(np.random.randn(3, 2), index=dates, columns=['a', 'b'])

print(df)

#                    a         b
# 2016-01-01  0.078142  1.364630
# 2016-01-02 -0.438748 -1.151145
# 2016-01-03 -1.615047  0.388382
```



一个columns也是一个Series

```python
import numpy as np
import pandas as pd

ndarray = np.array([[1, 88, 88],
                    [1, 77, 66],
                    [2, 55, 55],
                    [2, 44, 44]])
df = pd.DataFrame(ndarray, columns=['A', 'B', 'C'])

row = df.A
print(type(row)) # <class 'pandas.core.series.Series'>
```



**查看columns的类型**

```python
print(df.dtypes)

>>> df.dtypes
A      int64
B    float64
C     object
D     object
E      int64
dtype: object
```



###选择



**选择一列**

```python
df['code']
```

### 

选多列

```python
df[['a','b']]
```



**选择一行**

- **使用row id**



- **根据某一个column来查询**

```python
df.loc[df['column_name'] == some_value]
```

[参考](https://stackoverflow.com/questions/17071871/select-rows-from-a-dataframe-based-on-values-in-a-column-in-pandas)



选一个cell

```python
sub_df.iloc[0]['A']
```

[参考](https://stackoverflow.com/questions/16729574/how-to-get-a-value-from-a-cell-of-a-dataframe)



### 遍历



```python
import numpy as np
import pandas as pd

dates = pd.date_range('20160101', periods=3)
df = pd.DataFrame(np.random.randn(3, 2), index=dates, columns=['a', 'b'])

print(df)
#                    a         b
# 2016-01-01  0.078142  1.364630
# 2016-01-02 -0.438748 -1.151145
# 2016-01-03 -1.615047  0.388382

for index, row in df.iterrows():
    print(type(index)) # <class 'pandas._libs.tslib.Timestamp'>
    print(index) # 2016-01-01 00:00:00
    print(type(row)) # <class 'pandas.core.series.Series'>
    print(row)
    # a   -1.256422
    # b    0.675589
    # Name: 2016-01-01 00:00:00, dtype: float64
```



[参考](https://blog.csdn.net/ls13552912394/article/details/79349809)



### 增加column



```python
color = ['Red','Blue','Orange','Red','White','White','Blue','Green','Green','Red']
df['color'] = color
```



**增加column并且指定default值**

```python
df['Name'] = 'abc'
```





### 删除column



drop函数默认删除行，列需要加axis = 1

```python
import pandas as pd

df = pd.DataFrame(
        {'col1': [1, 2], 'col2': [0.5, 0.75]}, index=pd.to_datetime(['2018-05-1', '2018-06-01']))
print(df)

d2 = df.drop(['col1'], axis=1)
print(d2)

#             col1  col2
# 2018-05-01     1  0.50
# 2018-06-01     2  0.75
#             col2
# 2018-05-01  0.50
# 2018-06-01  0.75
```



[官方](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.drop.html)



###  to_dict() 和 Python的dt转Pandas的dt



```python
import pandas as pd

df = pd.DataFrame(
        {'col1': [1, 2], 'col2': [0.5, 0.75]}, index=pd.to_datetime(['2018-05-1', '2018-06-01']))
print(df)
#             col1  col2
# 2018-05-01     1  0.50
# 2018-06-01     2  0.75

f = df.iloc[0]
print(f)
# col1    1.0
# col2    0.5
# Name: 2018-05-01 00:00:00, dtype: float64

# Python的dt转Pandas的dt
print(type(f.name)) # <class 'pandas._libs.tslib.Timestamp'>

ts = f.name.to_pydatetime()
print(type(ts))
print(ts) # <class 'datetime.datetime'>

d = f.to_dict()
d['timestamp'] = f.name
print(d)
```





### as_matrix()



```python
import pandas as pd

d = {'col1': [1, 2], 'col2': [3, 4]}
df = pd.DataFrame(data=d)
print(type(df))
print(df)

#    col1  col2
# 0     1     3
# 1     2     4

m = df.as_matrix()
print(type(m)) # <class 'numpy.ndarray'>
print(m)
# [[1 3]
#  [2 4]]
```



### empty属性



```python
import pandas as pd

df_empty = pd.DataFrame({'A' : []})
print(df_empty)
# Empty DataFrame
# Columns: [A]
# Index: []
print(df_empty.empty) # True
```



[官方教程](http://pandas.pydata.org/pandas-docs/version/0.18/generated/pandas.DataFrame.empty.html)



### 重命名column



```python
df = pd.DataFrame({"A": [1, 2, 3], "B": [4, 5, 6]})
print(df)
d2 = df.rename(index=str, columns={"A": "H", "B": "B"})
print(d2)

#    A  B
# 0  1  4
# 1  2  5
# 2  3  6
#    H  B
# 0  1  4
# 1  2  5
# 2  3  6
```



[官方教程](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.rename.html)

### 

### 排序



```python
df.sort_values("Angle", inplace=True, ascending=True)
```



[官方文档](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.sort_values.html)



### 读取和写入CSV

**读取CSV**



指定读取哪些columns

指定解析成哪种数据类型

```python
import pandas as pd

pd.read_csv(open(csv_name, mode='rb'), usecols=['code', 'name'], dtype={'code': str})
```



**导到CSV**

显式地指定编码格式

```python
df.to_csv('basics.csv', encoding='utf_8_sig')
```



显示DF占用的内存



```python
df.memory_usage(index=True).sum()
```



> 单位是字节





## Panel



[教程](https://www.yiibai.com/pandas/python_pandas_panel.html)



### 创建



从ndarray创建

```python
import pandas as pd
import numpy as np


n1 = np.array(range(12))
n1.shape = (4, 3)
print(n1)


n2 = np.array(range(20, 32))
n2.shape = (4, 3)
print(n2)

data = {'Item1': pd.DataFrame(n1),
        'Item2': pd.DataFrame(n2)}
p = pd.Panel(data)
print(p)
```



### 查询



```python
import pandas as pd
import numpy as np


n1 = np.array(range(12))
n1.shape = (4, 3)
print(n1)
# [[ 0  1  2]
#  [ 3  4  5]
#  [ 6  7  8]
#  [ 9 10 11]]

n2 = np.array(range(20, 32))
n2.shape = (4, 3)
print(n2)


# [[20 21 22]
#  [23 24 25]
#  [26 27 28]
#  [29 30 31]]

data = {'Item1': pd.DataFrame(n1),
        'Item2': pd.DataFrame(n2)}
p = pd.Panel(data)
print(p)

# <class 'pandas.core.panel.Panel'>
# Dimensions: 2 (items) x 4 (major_axis) x 3 (minor_axis)
# Items axis: Item1 to Item2
# Major_axis axis: 0 to 3
# Minor_axis axis: 0 to 2

print('[Items axis]: Item1')
print(p['Item1'])
print('[Items axis]: Item2')
print(p['Item2'])

# [Items axis]: Item1
#    0   1   2
# 0  0   1   2
# 1  3   4   5
# 2  6   7   8
# 3  9  10  11
# [Items axis]: Item2
#     0   1   2
# 0  20  21  22
# 1  23  24  25
# 2  26  27  28
# 3  29  30  31

print('[major_xs]: 1')
print(p.major_xs(1))
print('[minor_xs]: 0')
print(p.minor_xs(0))

#    Item1  Item2
# 0      3     23
# 1      4     24
# 2      5     25
# [minor_xs]: 0
#    Item1  Item2
# 0      0     20
# 1      3     23
# 2      6     26
# 3      9     29
```



### 导出到文件



h5



```python
import pandas as pd
import numpy as np


n1 = np.array(range(12))
n1.shape = (4, 3)
n2 = np.array(range(20, 32))
n2.shape = (4, 3)

data = {'Item1': pd.DataFrame(n1),
        'Item2': pd.DataFrame(n2)}
p = pd.Panel(data)

p.to_hdf('a_panel.h5', 'key')



p = pd.read_hdf('a_panel.h5', 'key')
print(p)

print('[Items axis]: Item1')
print(p['Item1'])
print('[Items axis]: Item2')
print(p['Item2'])


print('[major_xs]: 1')
print(p.major_xs(1))
print('[minor_xs]: 0')
print(p.minor_xs(0))
```



Excel

```python
p.to_excel('a_panel.xlsx')
```







# Matplotlib



[入门教程](http://codingpy.com/article/a-quick-intro-to-matplotlib/)



## 简单



```python
import matplotlib.pyplot as plt
import numpy as np

# 简单的绘图
x = np.linspace(0, 2 * np.pi, 50)
plt.plot(x, np.sin(x)) # 如果没有第一个参数 x，图形的 x 坐标默认为数组的索引
plt.show() # 显示图形
```





## 在一张图上绘制两个数据集

```python
x = np.linspace(0, 2 * np.pi, 50)
plt.plot(x, np.sin(x),
        x, np.sin(2 * x))
plt.show()
```



## 自定义样式



```python
# 自定义曲线的外观
x = np.linspace(0, 2 * np.pi, 50)
plt.plot(x, np.sin(x), 'r-o',
        x, np.cos(x), 'g--')
plt.show()
```



另外一种

```python
plt.plot(x, y, color='red', linewidth=1.0, linestyle='--')
```



## 子图



```python
import matplotlib.pyplot as plt
import numpy as np

# 使用子图
x = np.linspace(0, 2 * np.pi, 50)
plt.subplot(2, 1, 1) # （行，列，活跃区）
plt.plot(x, np.sin(x), 'r')
plt.subplot(2, 1, 2)
plt.plot(x, np.cos(x), 'g')
plt.show()
```



## 直方图



```python
import numpy as np
import matplotlib.pyplot as plt

# 直方图
x = np.random.randn(1000)
plt.hist(x, 50)
plt.show()
```



## 标题，坐标轴标记和图例



```python
import numpy as np
import matplotlib.pyplot as plt

# 添加标题，坐标轴标记和图例
x = np.linspace(0, 2 * np.pi, 50)
plt.plot(x, np.sin(x), 'r-x', label='Sin(x)')
plt.plot(x, np.cos(x), 'g-^', label='Cos(x)')
plt.legend() # 展示图例
plt.xlabel('Rads') # 给 x 轴添加标签
plt.ylabel('Amplitude') # 给 y 轴添加标签
plt.title('标题') # 添加图形标题
plt.show()
```



```python
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0, 2 * np.pi, 50)
y = np.random.randn(5000)

plt.subplot(2, 1, 1)

plt.plot(x, np.sin(x), 'b', label='Sin(x)')
plt.plot(x, np.sin(2 * x), 'g', label='Sin(2*x)')
plt.title('This is title')
plt.xlabel('time')
plt.ylabel('price')
plt.legend()

plt.subplot(2, 1, 2)
plt.hist(y, label='a')
plt.legend()
plt.title('This is title2')
plt.xlabel('time')
plt.ylabel('volume')

plt.show()
```





## 限制x/y轴的显示范围



```python
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0, 2*np.pi, 50)
y = np.sin(x)

plt.plot(x, y)

plt.xlim(3, 5)

plt.show()
```



## 设置X/Y轴的刻度



```python
import numpy as np
import matplotlib.pyplot as plt

x = np.linspace(0, 2*np.pi, 50)
y = np.sin(x)

plt.plot(x, y)

new_ticks = np.linspace(2, 5, 5)
print(new_ticks)
plt.xticks(new_ticks)

plt.yticks([-1, -0.75, -0.25, 0.22, 1],[r'$really\ bad$', r'$bad$', r'$normal$', r'$good$', r'$really\ good$'])

plt.show()
```



## 图例



调整位置

```python
import numpy as np
import matplotlib.pyplot as plt

# 添加标题，坐标轴标记和图例
x = np.linspace(0, 2 * np.pi, 50)

plt.plot(x, np.sin(x), 'r-x', label='Sin(x)')
plt.plot(x, np.cos(x), 'g-^', label='Cos(x)')

plt.legend(loc='upper right') # 位置

plt.show()
```



## 标注



[教程](https://morvanzhou.github.io/tutorials/data-manipulation/plt/2-3-axis1/)



# bcolz

Bcolz 是一款支持数据压缩的，列数存储软件。提供可压缩内存和磁盘的柱状分块数据容器。列存储允许有效地查询表，以及列添加和删除。它基于 NumPy ，并将其用作标准数据容器与 Bcolz 对象进行通信。

默认情况下，Bcolz 对象被压缩，不仅可以减少内存/磁盘存储，还可以提高 I / O 速度。压缩过程由 Blosc 在内部执行，Blosc 是针对二进制数据进行优化的高性能多线程压缩器。



[官方教程](https://github.com/Blosc/bcolz/blob/master/docs/tutorial_ctable.ipynb)

[API文档](http://bcolz.readthedocs.io/en/latest/reference.html)



**从bcolz读取dataframe**



```python
from bcolz import ctable

path_name = 'fundemtals_tushare.bcol'
with ctable(rootdir=path_name, mode='a') as ct:
    df = ct.todataframe()
    print(df)
```



**从dataframe生成bcolz**



```python
import bcolz
import pandas as pd


df = pd.read_csv(open("df.csv", 'rb'))
df = df[['code', 'name', 'outstanding', 'pe', 'pb',  'profit']]

ct = bcolz.ctable.fromdataframe(df, rootdir='dataframe.bcolz')
ct.flush()
```





创建空的ctable

```python
# 指定column的名字和类型
dtype = [('date', 'i4'), ('open', 'f8')]

with bcolz.zeros(0, dtype=dtype, rootdir="mydir/ct_disk2") as ct_disk2:
    for i in range(20):
        ct_disk2.append((i, i ** 2))
```



实用方法-插入Dataframe

```python
def df_to_bcolz(import_df, path_name, expected_len=None):
    if not os.path.exists(path_name):
        print('Creating ctable directory')
        if not expected_len:
            expected_len = len(import_df)
        ct = ctable.fromdataframe(import_df, expectedlen=expected_len, rootdir=path_name, mode='w')
    else:
        ct = ctable(rootdir=path_name, mode='a')
        ct_import = ctable.fromdataframe(import_df, expectedlen=len(import_df))
        ct.append(ct_import)
        del ct_import

    ct.flush()
    print('Updated ctable: ', path_name)
```



显示column的类型

```python
print(ct.dtype)

# [('code', '<U6'), ('name', '<U4'), ('pe', '<f8'), ('outstanding', '<f8'), ('pb', '<f8'), ('profit', '<f8'), ('date', '<U8')]
```



有用的记录

zipline的本地行情是写入到`bcolz`的格式的，它是底层使用`Blosc`库的基于列的数据库，至于为什么使用基于列的数据库，应该是与行情信息的特质有关，因为行情信息可以通过TradingCalendar和Bcolz的元信息进行索引，并且以时间顺序排列，而且是相同的类型，所以非常适合类似数组结构的存储方式，加之以Blosc的变态级别的压缩解压算法（使用CPU L1/L2缓存进行压缩/解压，平均速度超过了`memcpy`调用），所以对时间和空间上都可以做到比较优化的状态。

内部的索引结构大概抽象为：

![zipline](https://github.com/rainx/inside-zipline/raw/master/images/zipline_bcolzwriter.png)



