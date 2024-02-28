---
title: "FastAPI教程"
description: 
date: 2024-02-03T14:37:00+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---





[官网](https://fastapi.tiangolo.com/zh/)



**依赖**

Python 3.8 及更高版本

FastAPI 站在以下巨人的肩膀之上：

- [Starlette](https://www.starlette.io/) 负责 web 部分。
- [Pydantic](https://pydantic-docs.helpmanual.io/) 负责数据部分。



**安装**

```bash
pip install fastapi
pip install "uvicorn[standard]"
```



**Quick Start**

创建一个 `main.py` 文件

```python
from typing import Optional

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
async def root():
    return {"message": "Hello World"}

@app.get("/items/{item_id}")
def read_item(item_id: int, q: Optional[str] = None):
    return {"item_id": item_id, "q": q}
```

运行

```bash
uvicorn main:app --reload
```

查询

```http
http://127.0.0.1:8000/items/5?q=somequery
```

查看文档

```http
http://127.0.0.1:8000/docs
http://127.0.0.1:8000/redoc
```

