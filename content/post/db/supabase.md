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



# 创建项目







# 获取连接信息

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





# Vercel提供的数据库

- [Vercel KV](https://vercel.com/docs/storage/vercel-kv): Durable Redis
- [Vercel Postgres](https://vercel.com/docs/storage/vercel-postgres): Serverless SQL
- [Vercel Blob](https://vercel.com/docs/storage/vercel-blob): Large file storage
- [Vercel Edge Config](https://vercel.com/docs/storage/edge-config): Global, low-latency data store



# 对比Vercel Postgres

[Vercel Postgres vs Supabase?](https://www.reddit.com/r/nextjs/comments/13oksux/vercel_postgres_vs_supabase/)



### Vercel Postgres 优点

1. **与 Vercel 平台集成：** 作为 Vercel 产品，它与其他 Vercel 服务的无缝集成。

### Vercel Postgres 缺点

1. **扩展和功能有限：** 与 Supabase 相比，它缺乏某些 Postgres 扩展和功能。
2. **成本：** 对于大规模应用来说，它被认为更昂贵。
3. **测试阶段：** 根据讨论，它仍处于测试阶段，可能不适合生产环境。

### Supabase 优点

1. **更多功能，更低成本：** Supabase 以更低的价格提供更广泛的功能。
2. **开箱即用的认证：** 用户赞赏内置的认证功能，认为非常有效。
3. **可扩展性：** Supabase 支持各种扩展，如 PostGIS、TIMESCALEDB、PGROONGA、PG_CRON、PgVector 等，这些都已准备好并且易于启用。
4. **易用性：** 用户发现 Supabase 易于使用，文档和资源良好。
5. **社区支持：** Supabase 社区似乎活跃且支持。

### Supabase 缺点

1. **行级安全（RLS）的担忧：** 一些用户对 Supabase 中 RLS 的默认设置表示担忧，这需要仔细管理以确保表的安全。
2. **数据库直接访问：** Supabase 允许从浏览器/客户端直接访问数据库，这要求用户在保护表格方面要小心。

### 一般观察

1. **社区偏好：** Next.js 社区似乎更喜欢 Supabase，提到其功能和定价。
2. **适用性：** 在 Vercel Postgres 和 Supabase 之间的选择可能取决于项目的特定需求和规模。
3. **Supabase 适合初学者，Neon 适合高级用户：** 对于数据库管理新手来说，推荐使用 Supabase，而更高级的用户可能更喜欢 Neon（与 Vercel Postgres 相关）。

### 结论

Vercel Postgres 和 Supabase 都有各自的优点和缺点。选择主要取决于项目的具体要求，如对某些 Postgres 扩展的需求、预算限制以及与 Vercel 生态系统集成的期望程度。社区中 Supabase 似乎是更受欢迎的选择，特别是因为其功能丰富和性价比高。

# 价格



Pro套餐25美元一个月，增加的功能：

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

