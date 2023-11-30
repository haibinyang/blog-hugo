---
title: Python入门
date: 2018-04-28 10:09:09
tags:
- Python
- 教程
- 入门
---



# 学习资料



- 在线：廖雪峰的[在线教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000)，挺不错的
- 纸质书：《Core Python》



# 基础



## Python 版本历史



| 版本        | 时间   |
| ----------- | ------ |
| Python 3000 | 2008年 |
| Python 2.7  | 2010年 |
| Python 3.5  | 2015年 |

[参考](https://pythoncaff.com/topics/22/the-version-history-of-python)



## 2.x和3.x的差异

[链接](http://www.runoob.com/python/python-2x-3x.html)



# 内置类型



## 判断类型



```python
# Variables of different types
i = 1
f = 0.1
s = "Hell"
l = [0, 1, 2]
d = {0: "Zero", 1: "One"}
t = (0, 1, 2)
b = True
n = None

print(type(i))
print(isinstance(i, int))

print(type(f))
print(isinstance(f, float))

print(type(s))
print(isinstance(s, str))

print(type(l))
print(isinstance(l, list))

print(type(d))
print(isinstance(d, dict))

print(type(t))
print(isinstance(t, tuple))

print(type(b))
print(isinstance(b, bool))

if n is None:
    print('n is None')
else:
    print('n is not None')
```

[参考](https://codeyarns.com/2010/01/28/python-checking-type-of-variable/)



支持列表

```python
# isinstance 支持tuple
print(isinstance(t, (list, tuple)))
```



## 特殊的值



### None

是一个对象

```python
print(type(None)) # <class 'NoneType'>
```



None和任何其他的数据类型比较永远返回False

- **if** 返回**False**


- **==**与其它类型比较，都是**False**



```python
a = 1
if a is None:
    print('True')
else:
    print('False')
# False
```



### Null

**Python** 没有这个关键字





### 无穷大

+∞（正无穷大）

-∞（负无穷大）

```python
float("inf")   # 正无穷
float("-inf")  # 负无穷
```



创建

```python
import math

a = float('inf')
print(type(a)) # <class 'float'>

c = math.inf
print(type(c)) # <class 'float'>
```



numpy也有方法

```
import numpy as np
np.inf
```



判断

```python
import math

b = math.inf
if math.isinf(b):
    print('True')
```



[链接](http://kuanghy.github.io/2016/12/03/python-inf-nan)



### NaN



> Not a Number



导致NaN的情况

- 无穷大减无穷大
- 无穷大 乘以 0 or 无穷小
- 除以无穷大



涉及到无穷大的四则运算, 若无法确定运算结果仍为无穷大, 那么运算结果就是一个NaN。



#### Python

创建

```python
a = float('nan')
print(type(a)) # <class 'float'>
print(a) # nan
```



判断

```python
import math

math.isnan(a)
```



NaN != NaN

b == b 结果为 False



#### NumPy

```
b = np.nan
print(type(b)) # <class 'float'>
print(b) # nan
```



[参考](https://blog.csdn.net/lanchunhui/article/details/50075217)



#### 产生NaN的情况



inf + 1还是inf

```python
import math

a = float('inf')

b = a + 1
print(type(b)) # <class 'float'>
print(b) # inf
```



inf - inf = nan

```python
import math

a = float('inf')

b = a - a
print(type(b)) # <class 'float'>
print(b) # nan
```



# 数字计算



1. /是精确除法，//是向下取整除法，%是求模 
2. %求模是基于向下取整除法规则的 
3. 四舍五入取整round, 向零取整int, 向下和向上取整函数math.floor, math.ceil 
4. //和math.floor在CPython中的不同 
5. /在python 2 中是向下取整运算 
6. C中%是向零取整求模



```python
print('usage of 3 operators /, // and % in python 3.4')
print('1). usage of /')
print ('10/4 = ', 10/4)
print ('-10/4 = ', -10/4)
print ('10/-4 = ', 10/-4)
print ('-10/-4 = ', -10/-4)

print('\n2). usage of //')
print ('10//4 = ', 10//4)
print ('-10//4 = ', -10//4)
print ('10//-4 = ', 10//-4)
print ('-10//-4 = ', -10//-4)

print('\n3). usage of %')
print ('10%4 = ', 10%4)
print ('-10%4 = ', -10%4)
print ('10%-4 = ', 10%-4)
print ('-10%-4 = ', -10%-4)
```



[参考](https://blog.csdn.net/huzq1976/article/details/51581330)



## 限制float的精度

```python
round(a, 2)
```





## 判断float是否相同



```python
import math

a = 5.0
b = 4.99998
math.isclose(a, b, rel_tol=1e-5)
# True
math.isclose(a, b, rel_tol=1e-6)
# False
```

[官网](https://docs.python.org/3/whatsnew/3.5.html#pep-485-a-function-for-testing-approximate-equality)



# 枚举类



```python
from enum import Enum

w = Enum('Week', ('Mon', 'Tue', 'Wed'))
print(type(w)) # <class 'enum.EnumMeta'>
print(w) # <enum 'week'>

# 使用某个值
print(w.Tue)
print(w.Tue.value)
print(w['Tue'])
print(w(2)) # 默认从1开始计数


print('***')
# 迭代
for name, member in w.__members__.items():
    print(name, '=>', member, ',', member.value)

# Mon => Week.Mon , 1
# Tue => Week.Tue , 2
# Wed => Week.Wed , 3
```



[参考](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/00143191235886950998592cd3e426e91687cdae696e64b000)



# 函数



## *args和**kwargs



- 这两个是python中的可变参数
- *args表示任何多个无名参数，它是一个tuple
- **kwargs表示关键字参数，它是一个dict

```python
def foo(*args, **kwargs):
    print 'args = ', args
    print 'kwargs = ', kwargs
    print '---------------------------------------'

if __name__ == '__main__':
    foo(1,2,3,4)
    foo(a=1,b=2,c=3)
    foo(1,2,3,4, a=1,b=2,c=3)
    foo('a', 1, None, a=1, b='2', c=3)
    
输出结果如下：
args =  (1, 2, 3, 4) 
kwargs =  {} 
--------------------------------------- 
args =  () 
kwargs =  {'a': 1, 'c': 3, 'b': 2} 
--------------------------------------- 
args =  (1, 2, 3, 4) 
kwargs =  {'a': 1, 'c': 3, 'b': 2} 
--------------------------------------- 
args =  ('a', 1, None) 
kwargs =  {'a': 1, 'c': 3, 'b': '2'} 
---------------------------------------
```

> 同时使用*args和**kwargs时，必须*args参数列要在**kwargs前，像foo(a=1, b='2', c=3, a', 1, None, )这样调用的话，会提示语法错误“SyntaxError: non-keyword arg after keyword arg”

[参考](http://www.cnblogs.com/fengmk2/archive/2008/04/21/1163766.html)



## 从命令行读取参数

```python
import sys

# main
param_1= sys.argv[1] 
param_2= sys.argv[2] 
param_3= sys.argv[3]  
print 'Params=', param_1, param_2, param_3
# 第一个参数是 Python文件的路径
# ['example.py', 'one', 'two', 'three']
```







## 装饰器

Python语言提供一个简单而强大的语法: ‘装饰器’。

装饰器是一个函数或类，它可以 包装(或装饰)一个函数或方法。被 ‘装饰’ 的函数或方法会替换原来的函数或方法。

 由于在Python中函数是一等对象，它也可以被 ‘手动操作’，但是使用@decorators 语法更清晰，因此首选这种方式。

**手动装饰**

```python
def foo():
    # 实现语句

def decorator(func):
    # 操作func语句
    return func

foo = decorator(foo)  # 手动装饰
```



**自动装饰**

```python
def log(func):
    def wrapper(*args, **kwargs):
        print('log')
        return func(*args, **kwargs)

    return wrapper

@log
def now():
    print('now')

now()
```

把`@log`放到`now()`函数的定义处，相当于执行了语句

```python
now = log(now)
```

[参考](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014318435599930270c0381a3b44db991cd6d858064ac0000)



# 实用函数



```python
isinstance(result, pd.Series)
```



# 打印



格式化

非常好的[文章](https://pyformat.info/)



```python
'%s %s' % ('one', 'two')
```

New

```python
'{} {}'.format('one', 'two')
```

Output

```
one two
```

Old

```python
'%d %d' % (1, 2)
```

New

```python
'{} {}'.format(1, 2)
```

Output

```
1 2
```



## 快速打印多个字符

```python
import six
six.print_("-" * 50)
```



## 格式



%f 表示 float



## 打印整个对象的属性



```python
from pprint import pprint
pprint(vars(self.__portfolio))
```



## 打印ndarray

直接打死ndarray，会使用科学计算法。需要设置下numpy。



```python
import numpy as np

np.set_printoptions(formatter={'float': '{: 0.3f}'.format})
```





# 字符串



## startswith

startswith takes `str or a tuple of str`

```python
>>> magicWord = 'zzzTest'
>>> magicWord.startswith(('zzz', 'yyy', 'rrr'))
True
```

[官方](https://docs.python.org/3/library/stdtypes.html#str.startswith)



## format



```python
>>> '{0}, {1}, {2}'.format('a', 'b', 'c')
'a, b, c'
>>> '{}, {}, {}'.format('a', 'b', 'c')  # 3.1+ only
'a, b, c'
>>> '{2}, {1}, {0}'.format('a', 'b', 'c')
'c, b, a'
>>> '{2}, {1}, {0}'.format(*'abc')      # unpacking argument sequence
'c, b, a'
>>> '{0}{1}{0}'.format('abra', 'cad')   # arguments' indices can be repeated
'abracadabra'
```



[参考](https://docs.python.org/3.4/library/string.html#format-examples)



## replace



```python
stock_id = stock_id.replace('.', '_')
```



[官方文档](https://docs.python.org/3/library/stdtypes.html#str.replace)



# 时间

[官方文档](https://docs.python.org/3/library/datetime.html)



## date对象



属性: [`year`](https://docs.python.org/3/library/datetime.html#datetime.date.year), [`month`](https://docs.python.org/3/library/datetime.html#datetime.date.month), and [`day`](https://docs.python.org/3/library/datetime.html#datetime.date.day).



### 转成字符串

```python
trade_date.isoformat()
# '2002-03-11'
```



[官方文档](https://docs.python.org/2/library/datetime.html#datetime.date.isoformat)



### 转成datetime对象



```python
from datetime import date
from datetime import datetime

d = date.today()
datetime.combine(d, datetime.min.time())
```





## time对象

Attributes: [`hour`](https://docs.python.org/3/library/datetime.html#datetime.time.hour), [`minute`](https://docs.python.org/3/library/datetime.html#datetime.time.minute), [`second`](https://docs.python.org/3/library/datetime.html#datetime.time.second), [`microsecond`](https://docs.python.org/3/library/datetime.html#datetime.time.microsecond), and [`tzinfo`](https://docs.python.org/3/library/datetime.html#datetime.time.tzinfo).



## datetime对象

Attributes: [`year`](https://docs.python.org/3/library/datetime.html#datetime.datetime.year), [`month`](https://docs.python.org/3/library/datetime.html#datetime.datetime.month), [`day`](https://docs.python.org/3/library/datetime.html#datetime.datetime.day), [`hour`](https://docs.python.org/3/library/datetime.html#datetime.datetime.hour), [`minute`](https://docs.python.org/3/library/datetime.html#datetime.datetime.minute), [`second`](https://docs.python.org/3/library/datetime.html#datetime.datetime.second), [`microsecond`](https://docs.python.org/3/library/datetime.html#datetime.datetime.microsecond), and [`tzinfo`](https://docs.python.org/3/library/datetime.html#datetime.datetime.tzinfo).

可以看成：`datetime对象`=`date对象`+`time对象`



###now()



```python
import datetime.datetime

ModuleNotFoundError: No module named 'datetime.datetime'; 'datetime' is not a package
```



```python
from datetime import datetime

print(datetime.now())
```



[官方文档](https://docs.python.org/3/library/datetime.html#datetime.datetime.now)



> 还有`today()`方法



### date()



转成date对象

[官方文档](https://docs.python.org/3/library/datetime.html#datetime.datetime.date)



### date转str：strftime()



```python
from datetime import datetime

dt = datetime(2015, 4, 19, 12, 20)
print(type(dt))
print(dt)

str = dt.strftime('%Y-%m-%d')
print(type(str))
print(str)

# <class 'datetime.datetime'>
# 2015-04-19 12:20:00
# <class 'str'>
# 2015-04-19
```



[字符串-格式文档](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior)



### replace()



```python
dt.replace(hour=0, minute=0, second=0, microsecond=0)
```

返回一个新的datetime对象。



[官方文档](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior)



## timedelta对象

初始化

`datetime.``timedelta`([*days*[, *seconds*[, *microseconds*[, *milliseconds*[, *minutes*[, *hours*[, *weeks*]]]]]]])



可以加减乘除

```python
import datetime

day = datetime.timedelta(days=1, seconds=40, milliseconds=10)
print(day)
print(day.days)
print(day.seconds)
print(day.microseconds)

ten_day = 10 * day
print(ten_day)
print(ten_day.days)
print(ten_day.seconds)
print(ten_day.microseconds)

d1 = ten_day / 10
print(d1)
print(d1.days)
print(d1.seconds)
print(d1.microseconds)
```



内置只有3个值

| Attribute      | Value                                      |
| -------------- | ------------------------------------------ |
| `days`         | Between -999999999 and 999999999 inclusive |
| `seconds`      | Between 0 and 86399 inclusive              |
| `microseconds` | Between 0 and 999999 inclusive             |

有3个常量

- `timedelta.``min


- `timedelta.``max


- `timedelta.``resolution


[官方](https://docs.python.org/2/library/datetime.html#timedelta-objects)



### milliseconds和microseconds

1000 milli second = 1 second

1 000 000 micro second = 1 second

```python
timedelta(milliseconds=10)
timedelta(microseconds=1000)
```



### 可以与datetime可以做加减

N/A



## 判断是否是同一天



思路：使用datetime.date()方法变成date类型



```python
from datetime import datetime

d1 = datetime(2018, 5, 13, 13, 2, 1)
d2 = datetime(2018, 5, 15, 13, 2, 1)
print(type(d1))
print(d1)

if d1 == d2:
    print('d1, d2 equal')
else:
    print('d1, d2 not equal')

d5 = d1.date()
print(type(d5))
print(d5)

if d1.date() == d2.date():
    print('d1, d2, equal')
else:
    print('d1, d2, not equal')
```



## 字符串转成datetime



```python
datetime.strptime(str(ss[0]), '%Y%m%d%H%M%S')
```



### 格式

见[官方文档](https://docs.python.org/3/library/datetime.html#strftime-strptime-behavior)



## Pandas的timestamp转datetime



```python
<class 'pandas._libs.tslib.Timestamp'>
index.to_pydatetime()
```



## timestamp

[三种方式](http://timestamp.online/article/how-to-get-current-timestamp-in-python)

```python
import datetime;

ts = datetime.datetime.now().timestamp()
print(ts)
# 1528679931.43
```



[转换工具](http://tool.chinaz.com/Tools/unixtime.aspx)



# 数据结构



## 列表



**逆序**

```python
list(reversed(a_list))
```





## 字典



方法

[参考](https://www.programiz.com/python-programming/methods/dictionary)

| Method                                                       | Description                                        |
| ------------------------------------------------------------ | -------------------------------------------------- |
| [Python Dictionary clear()](https://www.programiz.com/python-programming/methods/dictionary/clear) | Removes all Items                                  |
| [Python Dictionary copy()](https://www.programiz.com/python-programming/methods/dictionary/copy) | Returns Shallow Copy of a Dictionary               |
| [Python Dictionary fromkeys()](https://www.programiz.com/python-programming/methods/dictionary/fromkeys) | creates dictionary from given sequence             |
| [Python Dictionary get()](https://www.programiz.com/python-programming/methods/dictionary/get) | Returns Value of The Key                           |
| [Python Dictionary items()](https://www.programiz.com/python-programming/methods/dictionary/items) | returns view of dictionary's (key, value) pair     |
| [Python Dictionary keys()](https://www.programiz.com/python-programming/methods/dictionary/keys) | Returns View Object of All Keys                    |
| [Python Dictionary popitem()](https://www.programiz.com/python-programming/methods/dictionary/popitem) | Returns & Removes Element From Dictionary          |
| [Python Dictionary setdefault()](https://www.programiz.com/python-programming/methods/dictionary/setdefault) | Inserts Key With a Value if Key is not Present     |
| [Python Dictionary pop()](https://www.programiz.com/python-programming/methods/dictionary/pop) | removes and returns element having given key       |
| [Python Dictionary values()](https://www.programiz.com/python-programming/methods/dictionary/values) | returns view of all values in dictionary           |
| [Python Dictionary update()](https://www.programiz.com/python-programming/methods/dictionary/update) | Updates the Dictionary                             |
| [Python any()](https://www.programiz.com/python-programming/methods/built-in/any) | Checks if any Element of an Iterable is True       |
| [Python all()](https://www.programiz.com/python-programming/methods/built-in/all) | returns true when all elements in iterable is true |
| [Python ascii()](https://www.programiz.com/python-programming/methods/built-in/ascii) | Returns String Containing Printable Representation |
| [Python bool()](https://www.programiz.com/python-programming/methods/built-in/bool) | Coverts a Value to Boolean                         |
| [Python dict()](https://www.programiz.com/python-programming/methods/built-in/dict) | Creates a Dictionary                               |
| [Python enumerate()](https://www.programiz.com/python-programming/methods/built-in/enumerate) | Returns an Enumerate Object                        |
| [Python filter()](https://www.programiz.com/python-programming/methods/built-in/filter) | constructs iterator from elements which are true   |
| [Python iter()](https://www.programiz.com/python-programming/methods/built-in/iter) | returns iterator for an object                     |
| [Python len()](https://www.programiz.com/python-programming/methods/built-in/len) | Returns Length of an Object                        |
| [Python max()](https://www.programiz.com/python-programming/methods/built-in/max) | returns largest element                            |
| [Python min()](https://www.programiz.com/python-programming/methods/built-in/min) | returns smallest element                           |
| [Python map()](https://www.programiz.com/python-programming/methods/built-in/map) | Applies Function and Returns a List                |
| [Python sorted()](https://www.programiz.com/python-programming/methods/built-in/sorted) | returns sorted list from a given iterable          |
| [Python sum()](https://www.programiz.com/python-programming/methods/built-in/sum) | Add items of an Iterable                           |
| [Python zip()](https://www.programiz.com/python-programming/methods/built-in/zip) | Returns an Iterator of Tuples                      |



###get()

如果没有返回None



### 遍历



```python
d = {'k1':'v1', 'k2':'v2'}

print("############################")
items = d.items()
print(type(items)) # <class 'dict_items'>
print(items) # dict_items([('title', 't'), ('url', 'baidu.com')])

print("############################")
for key, value in items:
    print(type(key), type(value))
    print(key, value)

print("############################")
keys = d.keys()
print(type(keys)) # <class 'dict_keys'>
print(keys) # dict_keys(['title', 'url'])
for key in d.keys():
    print(key)

print("############################")
values = d.values()
print(type(values)) # <class 'dict_values'>
print(values) # dict_values(['t', 'baidu.com'])
for value in d.values():
    print(value)
```



## 集合

判断元素是否在集合中。

| Operation                   | Equivalent | Result                                                  |
| --------------------------- | ---------- | ------------------------------------------------------- |
| `len(s)`                    |            | number of elements in set *s* (cardinality)             |
| `x in s`                    |            | test *x* for membership in *s*                          |
| `x not in s`                |            | test *x* for non-membership in *s*                      |
| `s.issubset(t)`             | `s <= t`   | test whether every element in *s* is in *t*             |
| `s.issuperset(t)`           | `s >= t`   | test whether every element in *t* is in *s*             |
| `s.union(t)`                | `s | t`    | new set with elements from both *s* and *t*             |
| `s.intersection(t)`         | `s & t`    | new set with elements common to *s* and *t*             |
| `s.difference(t)`           | `s - t`    | new set with elements in *s* but not in *t*             |
| `s.symmetric_difference(t)` | `s ^ t`    | new set with elements in either *s* or *t* but not both |
| `s.copy()`                  |            | new set with a shallow copy of *s*                      |



[官网](https://docs.python.org/2/library/sets.html)



# 类



## 抽象基类



默认情况下，Python解析器不强制检查对抽象类的继承，即抽象类的子类可能没有实现其中的抽象方法，但是Python并不会报错。 为了避免这种情况，从Python 3.4/2.6开始，Python标准库中提供了abc模块（Abstract Base Classes），为定义Python的抽象基类提供了公共基础。 事实上，Python标准库中的numbers模块和collections模块都是abc模块的典型应用。



```python
from abc import ABC, abstractmethod


class Animal(ABC):

    @abstractmethod
    def run(self):
        pass


class Cat(Animal):
    def run(self):
        print('run')


# a = Animal() a = Animal() TypeError: Can't instantiate abstract class Animal with abstract methods run
c = Cat()
c.run()
```



[参考1](https://blog.csdn.net/taiyangdao/article/details/78199623)

[参考2](http://python3-cookbook.readthedocs.io/zh_CN/latest/c08/p12_define_interface_or_abstract_base_class.html)



## 属性



**手动包装**



```python
class Rect(object):
    def __init__(self):
        self.width = 0
        self.height = 0

    def setSize(self, size):
        self.width, self.height = size

    def getSize(self):
        return self.width, self.height

    size = property(getSize, setSize)

r = Rect()
r.width = 1
r.height = 2

s = r.getSize()
print(type(s)) # <class 'tuple'>
print(s)

r.size = 3, 5
print(r.size)
```



**装饰器**

把一个getter方法变成属性，只需要加上@property就可以了，此时，@property本身又创建了另一个装饰器@score.setter，负责把一个setter方法变成属性赋值，于是，我们就拥有一个可控的属性操作。

```python
class Animal(object):
    @property
    def name(self):
        return self._name

    @name.setter
    def set_name(self, value):
        self._name = value


a = Animal()
a.name = 'Peter'
print(a.name)
```



[参考1](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/00143186781871161bc8d6497004764b398401a401d4cce000)



## 继承



### 调用父类的方法



```python
class Animal(object):
    def __init__(self):
        print('Animal init')


class Cat(Animal):
    def __init__(self):
        super().__init__()
        print('Cat init')


c = Cat()
```



[官方文档](https://docs.python.org/3/library/functions.html#super)





## 静态方法



```python
class A(object):
    bar = 1
    def foo(self):
        print('foo')

    @staticmethod
    def static_foo():
        print('static_foo')
        print(A.bar)

    @classmethod
    def class_foo(cls):
        print('class_foo')
        print(cls.bar)
        cls().foo()


A.static_foo()
A.class_foo()
```



## 定制类



### \_\_str\_\_



```python
class Student(object):
    def __init__(self, name):
        self.name = name


class Student2(object):
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return 'Student2 object name=%s' % self.name



class Student3(object):
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return 'Student2 object name=%s' % self.name

    __repr__ = __str__


s1 = Student('Ken')
print(s1)

s2 = Student2('Peter') # <__main__.Student object at 0x100aead30>
print(s2) # Student2 object name=Peter
print(s2.__repr__()) # <__main__.Student2 object at 0x100f29e80>
```



`__repr__()`是为调试服务的



## 单例



```python
class Singleton(object):
    def __new__(cls, *args, **kw):
        if not hasattr(cls, '_instance'):
            orig = super(Singleton, cls)
            cls._instance = orig.__new__(cls, *args, **kw)
        return cls._instance


class MyClass(Singleton):
    a = 1


one = MyClass()
two = MyClass()

two.a = 3
print(one.a) # 3
print(id(one)) # 29097904
print(id(two)) # 29097904
print(one == two) # True
print(one is two) # True
```



>  将Singleton放到utils包的\_\_init\_\_文件中，就可以复用



## 对重载的支持-overloading

[见这里的讨论](https://www.zhihu.com/question/20053359)



## 获取实例的类名



```python
type(user).__name__
```



# 模块和包



[教程1](https://www.genedock.com/blog/2015/10/25/python-import/)

[教程2](http://www.runoob.com/python/python-modules.html)



## 基础

| 英文名  | 中文名 | 说明                            |
| ------- | ------ | ------------------------------- |
| Module  | 模块   | 一个Python文件                  |
| Package | 包     | 一个包含__init__.py文件的文件夹 |



定义模块

```python
# 面向 from mymodule import *
__all__ = ['sing', 'Animal']


# 方法1
def sing():
    print('sing')


# 类
class Animal(object):
    def run(self):
        print('animal run')


# 方法2
def walk():
    print('walk')
```



## 引用-import module格式

使用方式

```python
模块名.函数名
```

```python
import mymodule

mymodule.sing()

a = mymodule.Animal()
a.run()

mymodule.walk()
```



## 引用-from module import name 格式

指定具体引进的内容

```python
from mymodule import Animal

a = Animal()
a.run()

# sing() # 错误
```



## 引用-from module import * 格式

引进所有的内容

> 任何不以“__”开头的东西都被导入

```python
from mymodule import *

sing()

a = Animal()
a.run()

# walk() # NameError: name 'walk' is not defined
```



[参考](http://www.runoob.com/python/python-modules.html)



## 例子

文件结构

```bash
main.py
mymodule.py
utils
   __init__.py
   net.py
```



使用

```python
import mymodule
from utils.net import hi

a = mymodule.Animal()
a.run()
hi()
```



## 搜索路径

1、当前目录

2、如果不在当前目录，Python 则搜索在 shell 变量 PYTHONPATH 下的每个目录。

3、如果都找不到，Python会察看默认路径。UNIX下，默认路径一般为/usr/local/lib/python/。

 

## PYTHONPATH 变量

作为环境变量，PYTHONPATH 由装在一个列表里的许多目录组成。



## dir()函数

dir() 函数一个排好序的字符串列表，内容是一个模块里定义过的名字。

```python
import mymodule

print(dir(mymodule))
# ['Animal',
# '__all__', '__builtins__', '__cached__', '__doc__', '__file__', '__loader__', '__name__', '__package__', '__spec__',
# 'sing', 'walk']

print(mymodule.__name__) # mymodule
print(mymodule.__package__) # 内容为空
print(mymodule.__doc__) # None
print(mymodule.__file__) # /Users/yanghaibin/PycharmProjects/CorePython/Beginning/module/mymodule.py

print(globals())

# {
#     '__name__': '__main__',
#     '__doc__': None,
#     '__package__': None,
#     '__loader__': <_frozen_importlib_external.SourceFileLoader object at 0x10b7b5fd0>,
#     '__spec__': None,
#     '__annotations__': {},
#     '__builtins__': <module 'builtins' (built-in)>,
#     '__file__': '/Users/yanghaibin/PycharmProjects/CorePython/Beginning/module/dir.py',
#     '__cached__': None,
#     'mymodule': <module 'mymodule' from '/Users/yanghaibin/PycharmProjects/CorePython/Beginning/module/mymodule.py'>
# }
```

- `__name__`模块的名字
- `__file__`该模块的导入文件名



## 两种引用机制



- 相对引用：relative import
- 完全引进：absolute import

Python 3.x默认并且推荐使用完全引用。

[参考](https://www.genedock.com/blog/2015/10/25/python-import/)



## 完全引用



```python
from pkg import foo
from pkg.moduleA import foo
```



需要将pkg放到`PYTHONPATH`才能解析出来。



**PyCharm设置**

需要将模块和包设置为source，IDE才能识别成源码，进行说到底解析。

`Pycharm` `->` `Preferences` `->` `Project: XYZ` `->` `Project Structure`



[参考](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014318447437605e90206e261744c08630a836851f5183000)

[参考](https://blog.csdn.net/pwc1996/article/details/52577148)



### 相同项目模块间引用



- 使用完全引用
- 不要用相对路径，不然之后调整结构很麻烦，统一从项目根目录开始 import ，然后开发的时候把项目根目录加到 python 模块搜索路径



### 将模块或包加入PythonPath

PyCharms的方法

![](https://i.stack.imgur.com/sUjFS.png)



[参考](https://chrisyeh96.github.io/2017/08/08/definitive-guide-python-imports.html)

[参考](http://blog.51cto.com/bruceweien/1932422)



## 互相引用

解决办法

- 在函数中执行导入操作
- 在文件末尾执行导入操作



[参考](https://mozillazg.com/2014/06/python-import-xy.html)



# 异常



```python
try:
    lst = [1, 2, 3, 4, 5, 6]
    print((lst.index(7)))
except Exception as e:
    print(e)
    raise TypeError('hb')
else:
    print('good')
```



## 内置异常类



TypeError、ValueError、NameError、IndexError和KeyError可能经常使用。



```python
BaseException
 +-- SystemExit
 +-- KeyboardInterrupt
 +-- GeneratorExit
 +-- Exception
      +-- StopIteration
      +-- StopAsyncIteration
      +-- ArithmeticError
      |    +-- FloatingPointError
      |    +-- OverflowError
      |    +-- ZeroDivisionError
      +-- AssertionError
      +-- AttributeError
      +-- BufferError
      +-- EOFError
      +-- ImportError
      |    +-- ModuleNotFoundError
      +-- LookupError
      |    +-- IndexError
      |    +-- KeyError
      +-- MemoryError
      +-- NameError
      |    +-- UnboundLocalError
      +-- OSError
      |    +-- BlockingIOError
      |    +-- ChildProcessError
      |    +-- ConnectionError
      |    |    +-- BrokenPipeError
      |    |    +-- ConnectionAbortedError
      |    |    +-- ConnectionRefusedError
      |    |    +-- ConnectionResetError
      |    +-- FileExistsError
      |    +-- FileNotFoundError
      |    +-- InterruptedError
      |    +-- IsADirectoryError
      |    +-- NotADirectoryError
      |    +-- PermissionError
      |    +-- ProcessLookupError
      |    +-- TimeoutError
      +-- ReferenceError
      +-- RuntimeError
      |    +-- NotImplementedError
      |    +-- RecursionError
      +-- SyntaxError
      |    +-- IndentationError
      |         +-- TabError
      +-- SystemError
      +-- TypeError
      +-- ValueError
      |    +-- UnicodeError
      |         +-- UnicodeDecodeError
      |         +-- UnicodeEncodeError
      |         +-- UnicodeTranslateError
      +-- Warning
           +-- DeprecationWarning
           +-- PendingDeprecationWarning
           +-- RuntimeWarning
           +-- SyntaxWarning
           +-- UserWarning
           +-- FutureWarning
           +-- ImportWarning
           +-- UnicodeWarning
           +-- BytesWarning
           +-- ResourceWarning
```



[官网](https://docs.python.org/3/library/exceptions.html)



# 环境信息



显示操作系统信息

```python
import os

print(os.name) # posix
print(os.uname()) # posix.uname_result(sysname='Darwin', nodename='yanghaibindeMacBook-Pro.local',
```

os.name: 如果是`posix`，说明系统是`Linux`、`Unix`或`Mac OS X`，如果是`nt`，就是`Windows`系统。



显示环境变量

```python
import os

print(os.environ)
print(os.environ.get('PATH'))
print(os.environ.get('x', 'default'))
```



# 文件输入输出



[教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431925324119bac1bc7979664b4fa9843c0e5fcdcf1e000)



## 检查



创建Path对象

```python
from pathlib import Path
my_file = Path("/path/to/file")
```



是否存在

```python
if my_file.exists():
    # path exists
```



是否是文件

```python
if my_file.is_file():
    # file exists
```



是否是目录

```python
if my_file.is_dir():
    # directory exists
```



[参考](https://stackoverflow.com/questions/82831/how-to-check-whether-a-file-exists)



## 路径

**显示当前文件夹路径**

```python
import os

print(os.path.abspath('.')) 
# /Users/yanghaibin/PycharmProjects/CorePython/Beginning/pickle
```



**获取Home路径**

```python
from pathlib import Path
home = str(Path.home())
```



**获取当前Python文件的路径**

```python
os.path.dirname(os.path.realpath(__file__))
```

[参考](https://stackoverflow.com/questions/4934806/how-can-i-find-scripts-directory-with-python)



**拼接路径**

> 如果有子目录，要分开，不要放在一起

```python
import os

dirname = os.path.dirname(__file__)
filename = os.path.join(dirname, 'relative', 'd.txt') # 要分开

print(dirname)
# /Users/yanghaibin/PycharmProjects/CorePython/Beginning/pickle
print(filename)
# /Users/yanghaibin/PycharmProjects/CorePython/Beginning/pickle/relative/d.txt
```



**取文件名或扩展名**

```python
import os

r = os.path.split('/Users/michael/testdir/file.txt')
print(type(r)) # <class 'tuple'>
print(r) # ('/Users/michael/testdir', 'file.txt')

r = os.path.splitext('/path/to/file.txt')
print(r) # ('/path/to/file', '.txt')
```

判断扩展名

```python
os.path.splitext(x)[1]=='.py'
```



**判断路径是文件还是目录**



```python
import os

r = os.listdir('.')
print(type(r)) # <class 'list'>
print(r) # ['path1.py', 'a.txt', 'folder1']

folders = [x for x in os.listdir('.') if os.path.isdir(x)]
print(folders) # ['folder1']
```



**显示路径下的所有文件和目录**



```python
os.listdir('.')
```



## 目录

**创建和删除目录**

```python
# 然后创建一个目录:
>>> os.mkdir('/Users/michael/testdir')
# 删掉一个目录:
>>> os.rmdir('/Users/michael/testdir')
```



将中间缺少的目录也创建

```python
import os

os.makedirs('/Users/michael/testdir')
```



[官方文档](https://docs.python.org/3/library/os.html#os.makedirs)



## 文件



[教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431917715991ef1ebc19d15a4afdace1169a464eecc2000)



### 打开和关闭文件



**使用try方式**

```python
import os

dir_name = os.path.abspath('.')
print(dir_name)
file_name = os.path.join(dir_name, 'data', 'd1.txt')
print(file_name)

try:
    f = open(file_name, 'r')
    print(f.read())
    print('f.close = ', f.closed) # False
except Exception as e:
    print(e)
finally:
    try:
        f
    except NameError:
        print("well, it WASN'T defined after all!")
    else:
        if f:
            f.close()


print('f.close = ', f.closed) # True
```



**使用with方式**

with 语句适用于对资源进行访问的场合，确保不管使用过程中是否发生异常都会执行必要的“清理”操作，释放资源，比如文件使用后自动关闭、线程中锁的自动获取和释放等。

```python
import os

root = os.path.abspath('.')
file_path = os.path.join(root, 'data', 'd1.txt')

a = None
with open(file_path, 'r') as f:
    a = f
    print(f.read())
```

即使有异常也会关闭

```python
with open("file.txt", "r") as f:
    content = f.read()
    # Python still executes f.close() even though an exception occurs
    1 / 0
```



[深入了解with](https://www.ibm.com/developerworks/cn/opensource/os-cn-pythonwith/index.html)



### 读文件



read(size)

readlines()



### 写文件

write()



### 删除文件(文件夹)

```python
import os
import shutil

os.remove() # will remove a file.
os.rmdir() # will remove an empty directory.
shutil.rmtree() # will delete a directory and all its contents.
```







### 重命名



```python
import os

os.rename('d2.txt', 'd2-a.txt')
```



### 复制

`shutil`模块提供了`copyfile()`的函数。



```python
import shutil, errno

def copyanything(src, dst):
    try:
        shutil.copytree(src, dst)
    except OSError as exc: # python >2.5
        if exc.errno == errno.ENOTDIR:
            shutil.copy(src, dst)
        else: raise
```





## 异常



找不到文件可以抛出如下的异常

```python
    if not is_path_exist(file_name_path):
        raise FileNotFoundError('file not exist: ' + file_name_path)
```



## 序列化



- 我们把变量从内存中变成可存储或传输的过程称之为序列化。
- 序列化在Python中叫pickling，在其他语言中也被称之为serialization，marshalling，flattening等等，都是一个意思.
- 序列化之后，就可以把序列化后的内容写入磁盘，或者通过网络传输到别的机器上。
- 反过来，把变量内容从序列化的对象重新读到内存里称之为反序列化，即unpickling。



### pickle、JSON、shelve对比



- 需要与外部系统交互时用JSON模块
- 少量的数据使用pickle
-  需要将大量Python数据持久化到本地磁盘文件或需要一些简单的类似数据库的增删改查功能时，可以考虑用shelve模块。

[参考](http://www.cnblogs.com/yyds/p/6563608.html)



### pickle

> pickle的性能比cpickle差，建议使用cpic

```python
import pickle


class User:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def show(self):
        print(self.name + "_" + str(self.age))


# 序列化到文件
user = User("Python", 20)
user.show()
f = open('user.pkl', 'wb')
pickle.dump(user, f)
f.close()

# 反序列化到内存
f = open('user.pkl', 'rb')
user1 = pickle.load(f)
f.close()
user1.show()
```

[教程](http://www.zengyuetian.com/?p=2709)



# 数据存储



## HF5







# 数据库-SQLite



[中文入门教程](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001432010494717e1db78cd172e4d52b853e7fd38d6985c000)



## 异常处理



```python
def execute_sql(query, *args):
    con = None
    data = None

    try:
        con = sqlite3.connect(DB_FILENAME)
        cur = con.cursor()
        cur.execute(query, tuple(args))
        data = cur.fetchall()
        if not data:
            con.commit()
    except sqlite3.Error as e:
        print("Database error: %s" % e)
    except Exception as e:
        print("Exception in _query: %s" % e)
    finally:
        if con:
            con.close()

    return data
```



[参考](https://www.programcreek.com/python/example/6844/sqlite3.Error)



## SQLite的数据表的类型



[官网](https://www.sqlite.org/lang_datefunc.html)



>  SQL浏览工具-[DB Browser for SQLite](https://sqlitebrowser.org/)







# 线程



## 获取线程信息



```python
import threading

print(threading.get_ident())
print(threading.current_thread().name)
```



[文档](https://my.oschina.net/lionets/blog/194577)

[英文](https://docs.python.org/3/library/threading.html#threading.get_ident)





# 网络请求



非常好的[库](http://docs.python-requests.org/en/latest/user/quickstart/#custom-headers)



```python
url = 'http://wjwtradingview.moduointerface.com/public/tradingView/history'

headers = {'X-Custom-UserAgent': 'vbourse/v1.1.0', 'Content-Type': 'application/json;charset=UTF-8'}

# ts_start = int(datetime(2018, 6, 11, 0, 0, 0).timestamp())

now = datetime.now()
start = now - timedelta(minutes=30)

ts_end = int(now.timestamp())
ts_start = int(start.timestamp())

payload = '''
{"symbol":"DDW-WCNY","resolution":"1","from":%d,"to":%d}
''' % (ts_start, ts_end)

r = requests.post(url, data=payload, headers=headers)

r.status_code
r.json()
```



# 其它



## 循环



range(1, 5)

包含1,但不包含5



## 三元表达式

```python
1000 if instrument.type == 'INDX' else 100
```



## 代码规范

谷歌的[代码规范](http://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/)



##sleep



```python
import time

time.sleep(1)
```

[参考文档](https://www.tutorialspoint.com/python/time_sleep.htm)



## PEP

Python Enhancement Proposals (PEPs)

[官网](https://www.python.org/dev/peps/)



## 结构化工程



```bash
.project_root/                      项目根目录         
|
├── app/                            app代码等(不限定，可根据实际情况命名和确定结构)
│   ├── .../
│   ├── .../
│   └── ...
├── config/                         配置文件夹
│   ├── config.ini                  配置文件（应该被版本库ignore掉）
│   └── config.ini.example          配置文件示例
├── static/                         静态文件夹
│   ├── css/                        
│   ├── img/
│   ├── js/
│   └── favicon.ico                 网站图标
├── README.md                       项目说明文件
├── requrements.txt                 项目依赖文件
├── TODO.md                         待完成项目
└── .gitignore                      版本库ignore文件
```



[参考](http://pythonguidecn.readthedocs.io/zh/latest/writing/structure.html)

[参考](https://python-guide.gitbooks.io/python-style-guide/content/project-guide/dir.html)



## 将Python 2转成3的工具

[链接](http://www.pythonconverter.com/)



# PyCharm



## 变量名称切换大小写

cmd+shift+U



## 自动生成函数的注释



- 将光标定位到函数名
- 按 ⌥⏎
- 弹出窗口

![image-20180507152655423](/var/folders/rq/5t95w1lx5gx1m5zgwmf5nyvw0000gn/T/abnerworks.Typora/image-20180507152655423.png)



[参考](https://www.jetbrains.com/help/pycharm/creating-documentation-comments.html)



##优化import列表



- On the main menu, choose Code | Optimize Imports.
- Press ⌃⌥O.



##查看数据库文件



[教程](https://blog.csdn.net/qq_36482772/article/details/53458400)



## 在Jenkins中配置项目



1、安装插件`ShiningPanda`



2、在全局工具中配置Python路径

C:\Users\SQ\Anaconda3\



3、构建项目

选择：Python Builder

Nature: Shell

Command: python main.py update_source_data



4、配置Home的位置

系统管理-->系统配置

http://smallquant.iask.in:10620/configure

增加环境变量：

key: home

value: C:\Users\SQ



查看环境变量

http://smallquant.iask.in:10620/systemInfo





# Python Shell





**显示当前的工作目录**

```python
import os
os.getcwd()

# 一般是当前的目录
```



**切换工作目录**

```python
os.chdir('/server/accesslogs')
```



**执行module**



demo.py

```python
print('welcome to demo')

if __name__ == '__main__':
    print('from main')
else:
    print('not from main')
```



导入

```python
>>> import demo

welcome to demo
not from main
```



执行

```python
>>> exec(open("demo.py").read())
welcome to demo
from main
```



在WingIDE中可以：

Python Shell右上角的Options --> Evalute demo.py



在Pychamrs中可以：

run demo.py



[参考](https://stackoverflow.com/questions/1027714/how-to-execute-a-file-within-the-python-interpreter)





# 文档注释





```python
def get_trade_data_list(self, start: date=None, end: date=None, delta: timedelta=timedelta(days=1), count: int=None) -> list:
    
"""获取历史行情数据

暂时只支持日级别

参数
---------------
start : datetime.date
        start date

end : datetime.date
        end date

delta : datetime.timedelta
        时间间隔，暂时只支持日级别

count : int
        number of bars, 按照交易日来计算

返回值
-----------------
list(TradingData)
    list of TradingData

示例
--------
>>> stock = StockDataManager().get_stock('002106.XSHE')
>>> td_list = stock.get_trade_data_list(count=3)
>>> for td in td_list:
>>>     log.info(td.get_format_string())
2018-07-19 5.78 5.69 5.81 5.65 5566130.0 0.007906434659090909
2018-07-20 5.7 5.77 5.81 5.67 4766773.0 0.006770984375
2018-07-23 5.75 5.85 5.89 5.74 5457424.0 0.007752022727272727
"""
```



[参考1](https://python-guide.gitbooks.io/python-style-guide/content/style-guide/comment_and_docs.html)

[参考2](https://blog.csdn.net/liang19890820/article/details/74264380)

[参考3](http://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/)