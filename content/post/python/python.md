---
title: "Python"
description: 
date: 2024-01-26T13:17:07+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# Python的版本



| 序号 | Python 版本 | 版本类型 | 发布日期   | 相关PEP链接                                         |
| ---- | ----------- | -------- | ---------- | --------------------------------------------------- |
| 1    | 3.13        | 预发布   | 2024-10-01 | [PEP 719](https://peps.python.org/pep-0719/)        |
| 2    | 3.12        | Bug修复  | 2023-10-02 | [PEP 693](https://www.python.org/dev/peps/pep-0693) |
| 3    | 3.11        | Bug修复  | 2022-10-24 | [PEP 664](https://www.python.org/dev/peps/pep-0664) |
| 4    | 3.10        | 安全更新 | 2021-10-04 | [PEP 619](https://www.python.org/dev/peps/pep-0619) |
| 5    | 3.9         | 安全更新 | 2020-10-05 | [PEP 596](https://www.python.org/dev/peps/pep-0596) |
| 6    | 3.8         | 安全更新 | 2019-10-14 | [PEP 569](https://www.python.org/dev/peps/pep-0569) |



# Conda

## 创建环境

```bash
conda create --name llamaindex python=3.12
conda activate llamaindex
```



# 安装/升级/查看安装包

安装

```bash
pip install langchain
```

升级

```bash
pip install --upgrade langchain
```

查看版本号

键入Python进入交互界面

```python
import langchain
print(langchain.__version__)
```



# VSCode格式化Python

待定

