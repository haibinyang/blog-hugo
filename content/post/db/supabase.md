---
title: "Supabase入门"
description: 
date: 2023-12-18T13:34:19+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---







# 价格

[官方文档](https://www.restack.io/docs/supabase-knowledge-supabase-cost-analysis)

[官方文档2](https://supabase.com/docs/guides/platform/org-based-billing)



## 实例类型和费用

| Size    | Cost Per Hour | Estimated Monthly Cost |
| ------- | ------------- | ---------------------- |
| Starter | $0.01344      | ~$10                   |
| Small   | $0.0206       | ~$15                   |
| Medium  | $0.0822       | ~$60                   |
| Large   | $0.1517       | ~$110                  |
| XL      | $0.2877       | ~$210                  |
| 2XL     | $0.562        | ~$410                  |
| 4XL     | $1.32         | ~$960                  |
| 8XL     | $2.562        | ~$1870                 |
| 12XL    | $3.836        | ~$2800                 |
| 16XL    | $5.12         | ~$3730                 |





| Plan          | Hourly Price USD | Monthly Price USD | CPU                    | Memory | Connections: Pooler | Recommended Maximum DB Size |
| ------------- | ---------------- | ----------------- | ---------------------- | ------ | ------------------- | --------------------------- |
| Micro/Starter | $0.01344         | ~$10              | 2-core ARM (shared)    | 1 GB   | 200                 | 10GB                        |
| Small         | $0.0206          | ~$15              | 2-core ARM (shared)    | 2 GB   | 200                 | 50GB                        |
| Medium        | $0.0822          | ~$60              | 2-core ARM (shared)    | 4 GB   | 200                 | 100GB                       |
| Large         | $0.1517          | ~$110             | 2-core ARM (dedicated) | 8 GB   | 300                 | 200GB                       |
| XL            | $0.2877          | ~$210             | 4-core ARM (dedicated) | 16 GB  | 700                 | 500GB                       |





## 免费套餐

- 可以创建2个组织
- 每个组织都能够免费运行一个Starter实例
- 实例在一周不活动后暂停



## [Pro套餐](https://supabase.com/pricing)

> 每个月有10美元的Credit，不会转到下个月。



25美元一个月，相比免费，增加的功能：

1. **不暂停项目**：与免费计划中项目在一周不活动后暂停不同，专业计划中的项目不会因为不活动而暂停。
2. **每日备份且保存7天**：专业计划提供更全面的数据保护，每天进行备份，并保留7天。
3. **增加的数据库空间**：专业计划包含8GB的数据库空间，相较于免费计划的500MB。
4. **增加的文件存储空间**：提供100GB的文件存储空间，远高于免费计划的1GB。
5. **增加的带宽**：提供250GB的带宽，相比免费计划的5GB。
6. **更大的文件上传限制**：支持最多5GB的文件上传，而免费计划限制为50MB。
7. **更多的月活跃用户**：支持最多100,000的月活跃用户，超过免费计划的50,000。
8. **更多的Edge Function调用**：包含最多2M的Edge Function调用，相比免费计划的500K。
9. **更多的并发实时连接**：支持最多500个并发实时连接，免费计划中为200个。
10. **更多的实时消息**：包含最多500万的实时消息，免费计划为200万。
11. **延长的日志保留时间**：日志保留时间为7天，相较于免费计划的1天。
12. **电子邮件支持**：提供电子邮件客户支持，而免费计划只提供社区支持。



| 特点              | 免费计划               | 专业计划                         |
| ----------------- | ---------------------- | -------------------------------- |
| 适用性            | 个人爱好项目和简单网站 | 生产应用程序，提供扩展选项       |
| 价格              | $0 / 月 / 组织         | 从 $25 / 月 / 组织起             |
| 组织数量限制      | 最多2个免费组织        | 无                               |
| API请求           | 无限                   | 无限                             |
| 社交OAuth提供者   | 有                     | 有                               |
| 数据库空间        | 最多500MB              | 8GB包含                          |
| 文件存储          | 最多1GB                | 100GB包含                        |
| 带宽              | 最多5GB                | 250GB包含                        |
| 文件上传          | 最多50MB               | 5GB包含                          |
| 月活跃用户        | 最多50,000             | 100,000包含                      |
| Edge Function调用 | 最多500K               | 2M包含                           |
| 并发实时连接      | 最多200                | 500包含                          |
| 实时消息          | 最多200万              | 500万包含                        |
| 日志保留          | 1天                    | 7天                              |
| 支持              | 社区                   | 电子邮件                         |
| 项目暂停          | 1周不活跃后暂停        | 无                               |
| 备份              | 无                     | 每天备份，保存7天                |
| 成本控制          | 不适用                 | 允许根据设置决定是否接受超额使用 |



## 定价方式

每个新 Supabase 项目的默认实例是 Starter Compute 实例，可以随时通过项目设置进行升级。

Supabase 计算定价基于运行时小时数，这意味着您需要为实例处于活动状态的时间付费，而**不是为暂停的项目付费**。



## 对比Vercel的数据库

- [Vercel KV](https://vercel.com/docs/storage/vercel-kv): Durable Redis
- [Vercel Postgres](https://vercel.com/docs/storage/vercel-postgres): Serverless SQL
- [Vercel Blob](https://vercel.com/docs/storage/vercel-blob): Large file storage
- [Vercel Edge Config](https://vercel.com/docs/storage/edge-config): Global, low-latency data store



**对比Vercel Postgres**

[Vercel Postgres vs Supabase?](https://www.reddit.com/r/nextjs/comments/13oksux/vercel_postgres_vs_supabase/)



**Vercel Postgres 优点**

1. **与 Vercel 平台集成：** 作为 Vercel 产品，它与其他 Vercel 服务的无缝集成。

Vercel Postgres 缺点

1. **扩展和功能有限：** 与 Supabase 相比，它缺乏某些 Postgres 扩展和功能。
2. **成本：** 对于大规模应用来说，它被认为更昂贵。
3. **测试阶段：** 根据讨论，它仍处于测试阶段，可能不适合生产环境。

Supabase 优点

1. **更多功能，更低成本：** Supabase 以更低的价格提供更广泛的功能。
2. **开箱即用的认证：** 用户赞赏内置的认证功能，认为非常有效。
3. **可扩展性：** Supabase 支持各种扩展，如 PostGIS、TIMESCALEDB、PGROONGA、PG_CRON、PgVector 等，这些都已准备好并且易于启用。
4. **易用性：** 用户发现 Supabase 易于使用，文档和资源良好。
5. **社区支持：** Supabase 社区似乎活跃且支持。

Supabase 缺点

1. **行级安全（RLS）的担忧：** 一些用户对 Supabase 中 RLS 的默认设置表示担忧，这需要仔细管理以确保表的安全。
2. **数据库直接访问：** Supabase 允许从浏览器/客户端直接访问数据库，这要求用户在保护表格方面要小心。

一般观察

1. **社区偏好：** Next.js 社区似乎更喜欢 Supabase，提到其功能和定价。
2. **适用性：** 在 Vercel Postgres 和 Supabase 之间的选择可能取决于项目的特定需求和规模。
3. **Supabase 适合初学者，Neon 适合高级用户：** 对于数据库管理新手来说，推荐使用 Supabase，而更高级的用户可能更喜欢 Neon（与 Vercel Postgres 相关）。

**结论**

Vercel Postgres 和 Supabase 都有各自的优点和缺点。选择主要取决于项目的具体要求，如对某些 Postgres 扩展的需求、预算限制以及与 Vercel 生态系统集成的期望程度。社区中 Supabase 似乎是更受欢迎的选择，特别是因为其功能丰富和性价比高。



# 常用操作

## 安装CLI

> [链接](https://supabase.com/docs/guides/cli/getting-started)

```bash
brew install supabase/tap/supabase
```







## 获取连接信息

获取连接信息

**Migration connection string**

```sql
postgresql://postgres:[YOUR-PASSWORD]@db.vtczmwiywtazaaanrfrg.supabase.co:5432/postgres
```

- 对应下图的序号4
- 运维：平时管理数据库，不用考虑性能
- 使用postgresql当协议名

**Application connection string**

```sql
postgres://postgres.vtczmwiywtazaaanrfrg:[YOUR-PASSWORD]@aws-0-us-west-1.pooler.supabase.com:6543/postgres?pgbouncer=true
```

- 对应下图的序号5
- 应用：需要考虑性能
- 使用postgres当协议名
- 主机名有aws的字眼
- supabase返回的值没有，是我手动添加：pgbouncer=true
- `pgbouncer=true` 表明连接应该通过 PgBouncer 进行管理。使用 PgBouncer 可以提高性能和资源效率，因为它减少了创建和维护每个新数据库连接所需的开销。它通过重用现有连接来实现这一点。



![image-20240107144757114](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240107144757114.png)



## 启用扩展

点击[这里](https://supabase.com/dashboard/project/_/database/tables)

搜索Vector

## 查看函数

1. 登录到你的 Supabase 项目。
2. 在侧边栏中，点击“数据库”来打开数据库管理页面。
3. 选择“模式”视图，在这里你可以浏览不同

<img src="https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240301184349762.png" alt="image-20240301184349762" style="zoom:50%;" />



## 安装Javascript SDK

> [SDK文档](https://supabase.com/docs/reference/javascript/installing)

```bash
npm install @supabase/supabase-js
```









# SupaBase与向量数据库/AI

[官网](https://supabase.com/docs/guides/ai)



## [索引](https://supabase.com/docs/guides/ai/vector-indexes)

Currently vectors with up to 2,000 dimensions can be indexed.

但OpenAI的text-embedding-3-large已经是3072维度了。


| 模型名称               | 描述                               | 向量大小 | 价格          |
| :--------------------- | :--------------------------------- | :------- | ------------- |
| text-embedding-3-large | 适合非英文的任务。                 | 3,072    | 比ada稍微多点 |
| text-embedding-3-small | text-embedding-ada-002的升级版本。 | 1,536    | 是ada的1/5    |
| text-embedding-ada-002 |                                    | 1,536    |               |



不需要索引的情况

There are a couple of cases where you might not need indexes:

- You have a small dataset and don't need to scale it.
- You are not expecting high amounts of vector search queries per second.
- You need to guarantee 100% accuracy.（需要准确性：看来索引会降低准确性）。



使用索引将导致准确性降低，因为您要用近似 （ANN） 搜索替换精确 （KNN） 搜索。

KNN是精确搜索。



Prefer `inner-product` to `L2` or `Cosine` distances if your vectors are normalized (like `text-embedding-ada-002`). 





# Supabase与LangChain

LangChain的[教程](https://js.langchain.com/docs/integrations/vectorstores/supabase)

SupbaBase[关于LangChain的文档](https://supabase.com/docs/guides/ai/langchain#hybrid-search)

LangChain支持[Supabase混合检索](https://js.langchain.com/docs/integrations/retrievers/supabase-hybrid)



## 配置数据库

[教程](https://supabase.com/docs/guides/ai/langchain#hybrid-search)

**两种方式：**

- 在Dashboard的SQL Editor中找到LangChain模板（如下）
- 使用SupaBase CLI工具执行SQL语句



<img src="https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240301180618778.png" alt="image-20240301180618778" style="zoom:50%;" />



**基本原理**

- 启用vector插件
- 创建数据库表documents
- 创建查询函数match_documents

```sql
-- Enable the pgvector extension to work with embedding vectors
create extension vector;

-- Create a table to store your documents
create table documents (
  id bigserial primary key,
  content text, -- corresponds to Document.pageContent
  metadata jsonb, -- corresponds to Document.metadata
  embedding vector(1536) -- 1536 works for OpenAI embeddings, change if needed
);

-- Create a function to search for documents
create function match_documents (
  ……
$$;
```



## 获取SupbaBase API Key

- **`NEXT_PUBLIC_SUPABASE_URL`**: 这是你的Supabase项目的API URL。你可以在项目仪表板的`Settings` -> `API`部分找到它。这个URL通常看起来像 `https://xyzcompany.supabase.co`。
- **`NEXT_PUBLIC_SUPABASE_ANON_KEY`**: 这是你的Supabase项目的匿名公钥。它允许未经身份验证的用户访问你的数据库，但只能访问你在表格权限中明确允许的数据。这个键同样可以在`Settings` -> `API`部分找到。
- **`SUPABASE_SERVICE_ROLE_KEY`**: 这是服务角色密钥，它提供了对你的Supabase项目的完全访问权限，应当小心使用。你也可以在`Settings` -> `API`部分找到它，但请注意，它是高权限的密钥，不应该公开或在客户端代码中使用。

<img src="https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240301180503434.png" alt="image-20240301180503434" style="zoom:50%;" />

## 安装库

```bash
pnpm add @supabase/supabase-js
pnpm add @langchain/openai @langchain/community
```



## 基本用法

```js
import { SupabaseVectorStore } from 'langchain/vectorstores/supabase'
import { OpenAIEmbeddings } from 'langchain/embeddings/openai'
import { createClient } from '@supabase/supabase-js'

const supabaseKey = process.env.SUPABASE_SERVICE_ROLE_KEY
if (!supabaseKey) throw new Error(`Expected SUPABASE_SERVICE_ROLE_KEY`)

const url = process.env.SUPABASE_URL
if (!url) throw new Error(`Expected env var SUPABASE_URL`)

export const run = async () => {
  const client = createClient(url, supabaseKey)

  const vectorStore = await SupabaseVectorStore.fromTexts(
    ['Hello world', 'Bye bye', "What's this?"],
    [{ id: 2 }, { id: 1 }, { id: 3 }],
    new OpenAIEmbeddings(),
    {
      client,
      tableName: 'documents',
      queryName: 'match_documents',
    }
  )

  const resultOne = await vectorStore.similaritySearch('Hello world', 1)

  console.log(resultOne)
}
```



## 混合搜索

> [官方教程](https://js.langchain.com/docs/integrations/retrievers/supabase-hybrid/)

在SupaBase增加函数

```sql
-- Create a function to keyword search for documents
create function kw_match_documents(query_text text, match_count int)
returns table (id bigint, content text, metadata jsonb, similarity real)
as $$

begin
return query execute
format('select id, content, metadata, ts_rank(to_tsvector(content), plainto_tsquery($1)) as similarity
from documents
where to_tsvector(content) @@ plainto_tsquery($1)
order by similarity desc
limit $2')
using query_text, match_count;
end;
$$ language plpgsql;
```



检索

```js
import { SupabaseHybridSearch } from "@langchain/community/retrievers/supabase";
import { OpenAIEmbeddings } from 'langchain/embeddings/openai'
import { createClient } from '@supabase/supabase-js'

const supabaseKey = process.env.SUPABASE_SERVICE_ROLE_KEY
const url = process.env.NEXT_PUBLIC_SUPABASE_URL

export const run = async () => {
    const client = createClient(url, supabaseKey)
    const embeddings = new OpenAIEmbeddings();

    const retriever = new SupabaseHybridSearch(embeddings, {
        client,
        similarityK: 2,
        keywordK: 2,
        tableName: "documents",
        similarityQueryName: "match_documents",
        keywordQueryName: "kw_match_documents",
    });

    const results = await retriever.getRelevantDocuments("hello bye");

    console.log(results);
}

run();
```



**响应**

```bash
[
  Document { pageContent: 'Bye bye', metadata: { id: 1 } },
  Document { pageContent: 'Hello world', metadata: { id: 2 } }
]
```





## Simple metadata filtering

https://supabase.com/docs/guides/ai/langchain?database-method=sql#simple-metadata-filtering

## Advanced metadata filtering

https://supabase.com/docs/guides/ai/langchain?database-method=sql#simple-metadata-filtering



# Supabase与Next.js

N/A
