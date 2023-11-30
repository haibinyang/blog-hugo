---
title: RQAlpha
date: 2018-05-03 21:56:10
tags:
- rqalpha
- 量化投资平台
---



# 当日历史行情



```python
def handle_bar(context, dict_bar):
    snapshot = current_snapshot('000001.XSHE')
```

返回Snapshot对象

| 属性           | 类型              | 注释                 |
| -------------- | ----------------- | -------------------- |
| order_book_id  | str               | 股票代码             |
| datetime       | datetime.datetime | 当前快照数据的时间戳 |
| open           | float             | 当日开盘价           |
| last           | float             | 当前最新价           |
| high           | float             | 截止到当前的最高价   |
| low            | float             | 截止到当前的最低价   |
| prev_close     | float             | 昨日收盘价           |
| volume         | float             | 截止到当前的成交量   |
| total_turnover | float             | 截止到当前的成交额   |

具体见[官方](https://www.ricequant.com/api/python/chn#data-methods-current_snapshot)



只能在日内交易阶段调用，否则会抛出异常。



```python
from rqalpha.api import *

def init(context):
    pass


def before_trading(context):
    log.info('before_trading')
    # snapshot = current_snapshot('000001.XSHE') # RuntimeError: current_snapshot 不能在 [日内交易前] 中调用。


def handle_bar(context, dict_bar):
    snapshot = current_snapshot('000001.XSHE')
    log.info(type(snapshot))

def after_trading(context):
    log.info('after_trading')
    # snapshot = current_snapshot('000001.XSHE') # RuntimeError: current_snapshot 不能在 [日内交易后] 中调用。
```





# Mod



```bash
# 查看当前安装的 Mod 列表及状态
$ rqalpha mod list
# 安装 Mod
$ rqalpha mod install xxx
# 卸载 Mod
$ rqalpha mod uninstall xxx
# 启用 Mod
$ rqalpha mod enable xxx
# 禁用 Mod
$ rqalpha mod disable xxx
```





## 定义MOD的名字

在项目 `rqalpha-mod-hello` 下新建 `setup.py`



```python
from pip.req import parse_requirements

setup(
    name='rqalpha-mod-hello',     #mod名
    version="0.1.0",
)
```





# 扩展API



[官方](http://rqalpha.readthedocs.io/zh_CN/latest/development/data_source.html#api)



[实例](https://github.com/ricequant/rqalpha/blob/develop/rqalpha/examples/extend_api/rqalpha_mod_extend_api_demo.py)



```
class ExtendAPIDemoMod(AbstractMod):
    def __init__(self):
        self._csv_path = None
        self._inject_api()

    def start_up(self, env, mod_config):
        self._csv_path = os.path.abspath(os.path.join(os.path.dirname(__file__), mod_config.csv_path))
        self._env = env

    def tear_down(self, code, exception=None):
        pass

    def _inject_api(self):
        from rqalpha import export_as_api
        from rqalpha.execution_context import ExecutionContext
        from rqalpha.const import EXECUTION_PHASE

        @export_as_api
        @ExecutionContext.enforce_phase(EXECUTION_PHASE.ON_INIT,
                                        EXECUTION_PHASE.BEFORE_TRADING,
                                        EXECUTION_PHASE.ON_BAR,
                                        EXECUTION_PHASE.AFTER_TRADING,
                                        EXECUTION_PHASE.SCHEDULED)
        def sm_add_listener(event, func):
            self._env.event_bus.add_listener(event, func)
```



# 配置文件



配置选项见[这里](http://rqalpha.readthedocs.io/zh_CN/latest/intro/run_algorithm.html#id2)



参数名全称的`-`要变成`_`

例如`disable-user-system-log`要变成`user_system_log_disabled`



更准确的要看源码：

/Users/yanghaibin/anaconda3/lib/python3.6/site-packages/rqalpha/mod.py

可以看到所有的选项

```python
if self._mod_config.output_file:
    with open(self._mod_config.output_file, 'wb') as f:
        pickle.dump(result_dict, f)

if self._mod_config.report_save_path:
    from .report import generate_report
    generate_report(result_dict, self._mod_config.report_save_path)

if self._mod_config.plot or self._mod_config.plot_save_file:
    from .plot import plot_result
    plot_result(result_dict, self._mod_config.plot, self._mod_config.plot_save_file)
```



# 链接



[文档](http://rqalpha.readthedocs.io/zh_CN/latest/intro/overview.html)

[Github](https://github.com/ricequant/rqalpha/)

[API文档](https://www.ricequant.com/api/python/chn#data-methods-history_bars)



# Windows环境安装RQAlpha 







## Anaconda



**安装**

- [下载](https://repo.anaconda.com/archive/Anaconda3-5.2.0-Windows-x86_64.exe)
- 安装时必须勾选添加path选项



**检查Python版本**

在命令行输入python，看是否进入Anaconda版本的Python



**修改Conda的源**



设置源

```shell
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
conda config --set show_channel_urls yes
```

> 清华已经关闭该镜像



也可以直接个性 ~/.condarc文件

```
channels:
  - https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
  - defaults
ssl_verify: true
show_channel_urls: true
```

[教程](https://zhuanlan.zhihu.com/p/30438444)



## pip



**修改源**

修改c:\users\xxx\pip\pip.ini，修改index-url至相应源

```ini
[global]
index-url = http://pypi.douban.com/simple
[install]
trusted-host=pypi.douban.com
```



[教程](https://www.jianshu.com/p/a502577dc12e)

[官方文档](http://pip.readthedocs.io/en/latest/user_guide/#config-file)



## easy_install



指定源

创建配置文件： 

- Windows下是在： ~\pydistutils.cfg 
- Linux下是在： $HOME/.pydistutils.cfg



```ini
[easy_install]
index-url=http://pypi.douban.com/simple
```







## setuptools&&cython



使用管理员权限打开CMD

```shell
pip install -U setuptools cython -i https://pypi.douban.com/simple
```



Logbook

下载[Logbook](https://www.lfd.uci.edu/~gohlke/pythonlibs/#Logbook)

安装

```
pip install Logbook‑1.3.3‑cp36‑cp36m‑win_amd64.whl
```



## Bcolz



安装 Visual Studio

依赖于VC++编译库





安装

```shell
easy_install bcolz==1.2.0
```



[官方安装文档](http://bcolz.readthedocs.io/en/latest/install.html)



```shell
pip install bcolz==1.2.0 -i https://pypi.douban.com/simple
```



验证

```python
python -c "import bcolz; bcolz.test()"
```





## RQAlpha

[官方教程](http://rqalpha.readthedocs.io/zh_CN/latest/intro/install.html)

> 不要升级pip版本，保持在9.x.x版本 



```
pip install -i https://pypi.douban.com/simple rqalpha
rqalpha version
rqalpha update_bundle
```



## logbook



pip uninstall logbook

pip install logbook==1.0.0



## TA-Lib

 

下载[TA-Lib包](http://prdownloads.sourceforge.net/ta-lib/ta-lib-0.4.0-msvc.zip)

解压到

```powershell
C:\ta-lib
```



下载[wheel包](https://download.lfd.uci.edu/pythonlibs/l8ulg3xw/TA_Lib-0.4.17-cp36-cp36m-win_amd64.whl)

安装

```python
pip install TA_Lib-0.4.9-cp27-none-win_amd64.whl
```



安装包

```
pip install TA-Lib
```





[Wheel官网](https://www.lfd.uci.edu/~gohlke/pythonlibs/#bcolz)

[TA-Lib安装指南](https://mrjbq7.github.io/ta-lib/install.html)





## 其它

**Tushare**

pip install tushare -i https://pypi.douban.com/simple





# Mac环境



## Redis



安装

[教程](https://blog.csdn.net/HobHunter/article/details/77877008)



运行

sudo redis-server 



检查是否运行成功

redis-cli ping



[教程](https://blog.csdn.net/pingpangbing0902/article/details/47104545)





命令工具

redis-cli

进行交互命令



常见命令

| keys *            | 查看所有键值              |
| ----------------- | ------------------------- |
| set key value     | 设置键key的值为value      |
| append key value2 | 在键key的值后面加上value2 |
| get key           | 查看键key的值             |



也可以直接使用

redis-cli get key



[教程](https://www.jianshu.com/p/af33284aa57a)



# 使用统一行情



```
# 启用 Mod
$ rqalpha mod enable sys_stock_realtime
```



```
rqalpha quotation_server redis://localhost/1
```



[参考](https://github.com/ricequant/rqalpha/blob/master/rqalpha/mod/rqalpha_mod_sys_stock_realtime/README.rst)

