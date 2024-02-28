---
title: "Poeptry教程"
description: 
date: 2024-02-03T14:37:00+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



[官网](https://python-poetry.org/)



**安装**

在Conda环境中安装（重要）

```bash
conda install poetry -y
```

查看版本

```bash
poetry --version
```



**创建项目**

```
poetry new poetry-demo
```



**已存在的项目**

初始化

```bash
poetry init
```



添加库

```bash
poetry add fastapi
```

```bash
poetry add "fastapi[all]"
```

