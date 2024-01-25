---
title: "向量数据库"
description: 
date: 2024-01-25T14:52:38+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# 时间线

Vespa 是最早在主流的基于 BM25 关键字搜索算法旁边加入向量相似性搜索的厂商之一（有趣的是，Vespa 的 GitHub 仓库 现在已经有近 75000 次提交 🤯）。

Weaviate 随后在2018年底推出了一个专门的开源向量搜索数据库产品。

到2019年，我们开始在这个领域看到更多的竞争，包括 Milvus（也是开源的）。需要注意的是，在时间线中也显示了 Zilliz，但没有单独列出，因为它是 Milvus 的（商业）母公司，提供了基于 Milvus 构建的完全托管的云解决方案。

在2021年，又有三家新的供应商加入了竞争：Vald、Qdrant 和 Pinecone。

直到此时，像 Elasticsearch、Redis 和 PostgreSQL 这样的老牌厂商才开始提供向量搜索，比人们原本想象的要晚得多，仅仅在2022年以及之后才开始。

![image-20240125145334529](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240125145334529.png)

# 开源和商业

商业：Pinecone和Zilliz

插件形式

- pgvector
- Redis Stack





![image-20240125145550416](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240125145550416.png)



# Postgres

一个数据库同时支持：

- 关系数据库：RDS
- 向量数据库：pgvector
- 时间序列数据库：时序数据库在元数据过滤中发挥了重大作用，它是一种记录事件和发生时间的数据库，对于时间序列的搜索速度非常快。在RAG应用中，如果行业知识文件被切分出几万个，那么使用时间过滤就会非常重要，比如我们只需要检索2023年3月份的合同文件，那么就可以用时序数据将目标chunk从几万个里面先挑出来，再进行向量计算。

<img src="https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240125150309961.png" alt="image-20240125150309961" style="zoom: 33%;" />

## Timescale Vector插件

对数百万个向量的更快的相似性搜索：支持**DiskANN**算法，**HNSW**算法

- **Timescale Vector优化了基于时间的向量搜索查询：**利用Timescale的超级表的自动基于时间的分区和索引，有效地找到最近的Embeddings，通过时间范围或文档存在年份约束向量搜索，并轻松存储和检索大型语言模型(LLM)响应和聊天历史。基于时间的语义搜索还使您能够使用**检索增强生成**(Retrieval Augmented Generation, **RAG**)和基于时间的上下文检索，从而为用户提供更有用的LLM响应。
- **简化的AI基础设施堆栈：**通过将**向量Embeddings**，**关系型数据**和**时间序列数据**组合在一个PostgreSQL数据库中，Timescale vector消除了大规模管理多个数据库系统所带来的操作复杂性。
- **简化元数据处理和多属性过滤：**开发人员可以利用所有PostgreSQL数据类型来存储和过滤元数据，并将向量搜索结果与关系数据连接起来，以获得更多上下文相关的响应。在未来的版本中，Timescale Vector将进一步优化丰富的多属性过滤，在过滤元数据时实现更快的相似性搜索。



# 参考

https://mp.weixin.qq.com/s/YENmch0b4rbNJ73bvBLUpQ


