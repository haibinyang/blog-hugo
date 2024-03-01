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

Vespa 是最早在主流的基于 BM25 关键字搜索算法旁边加入向量相似性搜索的厂商之一。

Weaviate 随后在2018年底推出了一个专门的开源向量搜索数据库产品。

到2019年，我们开始在这个领域看到更多的竞争，包括 Milvus（也是开源的）。Zilliz是 Milvus 的母公司。

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



# LlamaIndex整理的向量数据库

[链接](https://docs.llamaindex.ai/en/stable/module_guides/storing/vector_stores.html)



**Vector Store Options & Feature Support**

| Vector Store             | Type                | Metadata Filtering | Hybrid Search | Delete | Store Documents | Async |
| ------------------------ | ------------------- | ------------------ | ------------- | ------ | --------------- | ----- |
| Apache Cassandra®        | self-hosted / cloud | ✓                  |               | ✓      | ✓               |       |
| Astra DB                 | cloud               | ✓                  |               | ✓      | ✓               |       |
| Azure Cognitive Search   | cloud               |                    | ✓             | ✓      | ✓               |       |
| Azure CosmosDB MongoDB   | cloud               |                    |               | ✓      | ✓               |       |
| ChatGPT Retrieval Plugin | aggregator          |                    |               | ✓      | ✓               |       |
| Chroma                   | self-hosted         | ✓                  |               | ✓      | ✓               |       |
| DashVector               | cloud               | ✓                  | ✓             | ✓      | ✓               |       |
| Deeplake                 | self-hosted / cloud | ✓                  |               | ✓      | ✓               |       |
| DocArray                 | aggregator          | ✓                  |               | ✓      | ✓               |       |
| DynamoDB                 | cloud               |                    |               | ✓      |                 |       |
| Elasticsearch            | self-hosted / cloud | ✓                  | ✓             | ✓      | ✓               | ✓     |
| FAISS                    | in-memory           |                    |               |        |                 |       |
| txtai                    | in-memory           |                    |               |        |                 |       |
| Jaguar                   | self-hosted / cloud | ✓                  | ✓             | ✓      | ✓               |       |
| LanceDB                  | cloud               | ✓                  |               | ✓      | ✓               |       |
| Lantern                  | self-hosted / cloud | ✓                  | ✓             | ✓      | ✓               | ✓     |
| Metal                    | cloud               | ✓                  |               | ✓      | ✓               |       |
| MongoDB Atlas            | self-hosted / cloud | ✓                  |               | ✓      | ✓               |       |
| MyScale                  | cloud               | ✓                  | ✓             | ✓      | ✓               |       |
| Milvus / Zilliz          | self-hosted / cloud | ✓                  |               | ✓      | ✓               |       |
| Neo4jVector              | self-hosted / cloud |                    |               | ✓      | ✓               |       |
| OpenSearch               | self-hosted / cloud | ✓                  |               | ✓      | ✓               |       |
| Pinecone                 | cloud               | ✓                  | ✓             | ✓      | ✓               |       |
| Postgres                 | self-hosted / cloud | ✓                  | ✓             | ✓      | ✓               | ✓     |
| pgvecto.rs               | self-hosted / cloud | ✓                  | ✓             | ✓      | ✓               |       |
| Qdrant                   | self-hosted / cloud | ✓                  | ✓             | ✓      | ✓               | ✓     |
| Redis                    | self-hosted / cloud | ✓                  |               | ✓      | ✓               |       |
| Simple                   | in-memory           | ✓                  |               | ✓      |                 |       |
| SingleStore              | self-hosted / cloud | ✓                  |               | ✓      | ✓               |       |
| Supabase                 | self-hosted / cloud | ✓                  |               | ✓      | ✓               |       |
| Tair                     | cloud               | ✓                  |               | ✓      | ✓               |       |
| TencentVectorDB          | cloud               | ✓                  | ✓             | ✓      | ✓               |       |
| Timescale                |                     | ✓                  |               | ✓      | ✓               | ✓     |
| Typesense                | self-hosted / cloud | ✓                  |               | ✓      | ✓               |       |
| Weaviate                 | self-hosted / cloud | ✓                  | ✓             | ✓      | ✓               |       |



大部分支持的数据库

| ector Store     | Type                | Metadata Filtering | Hybrid Search | Delete | Store Documents | Async |                  |
| --------------- | ------------------- | ------------------ | ------------- | ------ | --------------- | ----- | ---------------- |
| DashVector      | cloud               | ✓                  | ✓             | ✓      | ✓               |       |                  |
| Elasticsearch   | self-hosted / cloud | ✓                  | ✓             | ✓      | ✓               | ✓     | 总觉得比较重     |
| Jaguar          | self-hosted / cloud | ✓                  | ✓             | ✓      | ✓               |       |                  |
| Lantern         | self-hosted / cloud | ✓                  | ✓             | ✓      | ✓               | ✓     |                  |
| MyScale         | cloud               | ✓                  | ✓             | ✓      | ✓               |       |                  |
| Pinecone        | cloud               | ✓                  | ✓             | ✓      | ✓               |       |                  |
| Postgres        | self-hosted / cloud | ✓                  | ✓             | ✓      | ✓               | ✓     |                  |
| pgvecto.rs      | self-hosted / cloud | ✓                  | ✓             | ✓      | ✓               |       |                  |
| Qdrant          | self-hosted / cloud | ✓                  | ✓             | ✓      | ✓               | ✓     | 创始人好像出走了 |
| TencentVectorDB | cloud               | ✓                  | ✓             | ✓      | ✓               |       |                  |
| Weaviate        | self-hosted / cloud | ✓                  | ✓             | ✓      | ✓               |       |                  |



[Elasticsearch](https://docs.llamaindex.ai/en/stable/examples/vector_stores/ElasticsearchIndexDemo.html)：总觉得比较重

[Postgress](https://docs.llamaindex.ai/en/stable/examples/vector_stores/postgres.html#hybrid-search)：先从最简单的开始吧。

[Qdrant](https://docs.llamaindex.ai/en/stable/examples/vector_stores/qdrant_hybrid.html)：创始人好像出走了。



# LangChain对数据库的对比

[原文](https://js.langchain.com/docs/modules/data_connection/vectorstores/#which-one-to-pick)

| 数据库名称                           | 应用场景                                                     |
| ------------------------------------ | ------------------------------------------------------------ |
| HNSWLib, Faiss, LanceDB, CloseVector | 如果你需要一个可以在你的Node.js应用程序中运行的内存数据库，无需其他服务器 |
| MemoryVectorStore, CloseVector       | 如果你在寻找一个可以在类似浏览器的环境中内存中运行的东西     |
| HNSWLib, Faiss                       | 如果你来自Python，并且你在寻找类似于FAISS的东西              |
| Chroma                               | 如果你在寻找一个开源的、功能全面的向量数据库，可以在docker容器中本地运行 |
| Zep                                  | 如果你在寻找一个开源的向量数据库，提供低延迟、本地嵌入文档支持，并且支持边缘上的应用 |
| Weaviate                             | 如果你在寻找一个开源的、生产就绪的向量数据库，可以在docker容器中本地运行或在云中托管 |
| Supabase vector store                | 如果你已经在使用Supabase，看看Supabase向量存储，使用同一个Postgres数据库来存储你的嵌入 |
| Pinecone                             | 如果你在寻找一个生产就绪的向量存储，你不必担心自己托管       |
| SingleStore vector store             | 如果你已经在使用SingleStore，或者你需要一个分布式、高性能的数据库，你可能会考虑SingleStore向量存储 |
| AnalyticDB vector store              | 如果你在寻找一个在线MPP（大规模并行处理）数据仓库服务，你可能会考虑AnalyticDB向量存储 |
| MyScale                              | 如果你在寻找一个性价比高的向量数据库，允许使用SQL进行向量搜索 |
| CloseVector                          | 如果你在寻找一个可以从浏览器和服务器端加载的向量数据库，看看CloseVector。它是一个旨在跨平台的向量数据库 |
| ClickHouse                           | 如果你在寻找一个可扩展的、开源的列式数据库，对于分析查询有着出色的性能 |



# 不同数据库的对比



[开源向量数据库对比](https://zilliz.com.cn/comparison)



# RAG选型



[Elasticsearch](https://docs.llamaindex.ai/en/stable/examples/vector_stores/ElasticsearchIndexDemo.html)

[Qdrant](https://docs.llamaindex.ai/en/stable/examples/vector_stores/qdrant_hybrid.html)

[Postgress](https://docs.llamaindex.ai/en/stable/examples/vector_stores/postgres.html#hybrid-search)





# 参考

https://mp.weixin.qq.com/s/YENmch0b4rbNJ73bvBLUpQ



