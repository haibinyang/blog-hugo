---
title: "LlamaIndex"
description: 
date: 2024-01-26T09:08:40+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# Python和Typescript版本



Python版本的文档更完善，ts比较差？



# 入门

[官方教程](https://docs.llamaindex.ai/en/stable/getting_started/starter_example.html)



## 创建环境

```bash
conda create --name llamaindex python=3.12
conda activate llamaindex
```



**在VSCode中设置**

Python: Select Interpreter



## 安装库

```bash
pip install llama-index
```



## 配置OpenAI

```bash
vim ~/.zshrc
```

添加环境变量

```bash
export OPENAI_API_KEY="sk-xxxx"
```

验证

```bash
echo $OPENAI_API_KEY
```



**可达性**

在命令行配置: goproxy



## Quick Start



```python
import os.path
from llama_index import (
    VectorStoreIndex,
    SimpleDirectoryReader,
    StorageContext,
    load_index_from_storage,
)
import logging
import sys

logging.basicConfig(stream=sys.stdout, level=logging.DEBUG)
logging.getLogger().addHandler(logging.StreamHandler(stream=sys.stdout))

# check if storage already exists
PERSIST_DIR = "./storage"
if not os.path.exists(PERSIST_DIR):
    # load the documents and create the index
    documents = SimpleDirectoryReader("data").load_data()
    index = VectorStoreIndex.from_documents(documents)
    # store it for later
    index.storage_context.persist(persist_dir=PERSIST_DIR)
else:
    # load the existing index
    storage_context = StorageContext.from_defaults(persist_dir=PERSIST_DIR)
    index = load_index_from_storage(storage_context)

# either way we can now query the index
query_engine = index.as_query_engine()
response = query_engine.query("What did the author do growing up?")
print(response)
```



**使用的completions方法**

```
/chat/completions
```



**查询的参数**

```json
{
  "messages": [
    {
      "role": "system",
      "content": "You are an expert Q&A system that is trusted around the world.\nAlways answer the query using the provided context information, and not prior knowledge.\nSome rules to follow:\n1. Never directly reference the given context in your answer.\n2. Avoid statements like \"Based on the context, ...\" or \"The context information ...\" or anything along those lines."
    },
    {
      "role": "user",
      "content": "xxx"
    }
  ],
  "model": "gpt-3.5-turbo",
  "stream": false,
  "temperature": 0.1
}
```



**System Prompt**

```bash
You are an expert Q&A system that is trusted around the world.
Always answer the query using the provided context information, and not prior knowledge.
Some rules to follow:
1. Never directly reference the given context in your answer.
2. Avoid statements like "Based on the context, ..." or "The context information ..." or anything along those lines.
```

> 您是一个受到全世界信赖的专家问答系统。 在回答问题时，始终使用所提供的背景信息，而不是先前的知识。 需要遵循的一些规则：
>
> 1. 永远不要在答案中直接引用给定的背景信息。
> 2. 避免使用“根据背景信息，…”或“背景信息表明，…”或任何类似的表述。



**User Prompt**

```bash
Context information is below.
---------------------
file_path: data/paul_graham_essay.txt

xxx
---------------------
Given the context information and not prior knowledge, answer the query.
Query: What did the author do growing up?
Answer:  
```





# Deeplearn教程

> Building and Evaluating Advanced RAG Applications：[链接](https://www.deeplearning.ai/short-courses/building-evaluating-advanced-rag/) [Bilibili](https://www.bilibili.com/video/BV1494y1E7H9?p=2&vd_source=a77fcba61aa907ebeb43a374b034f3b3)









## Joint Text to SQL and Semantic Search

This video covers the tools built into LlamaIndex for combining SQL and semantic search into a single unified query interface.

[Youtube](https://www.youtube.com/watch?v=ZIvcVJGtCrY)

[Notebook](https://docs.llamaindex.ai/en/stable/getting_started/discover_llamaindex.html#../../examples/query_engine/SQLAutoVectorQueryEngine.ipynb)



# Response Modes

[参考](https://docs.llamaindex.ai/en/stable/module_guides/deploying/query_engine/response_modes.html)



# TruLens

> [官网](https://www.trulens.org/)



## 评测标准

Query与Answer：Answer Relevance

Query与Context：Context Relevance

Answer与Context：Groundedness

> grounded：脚踏实地的；groundedness：有根性



![image-20240201110046813](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240201110046813.png)

