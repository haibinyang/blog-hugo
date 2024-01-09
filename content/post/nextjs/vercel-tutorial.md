---
title: "Vercel Tutorial"
description: 
date: 2023-12-16T13:32:59+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---





# 价格和限制

## 总结



| 分类（原始名称）                                             | 业余爱好者计划（Hobby） | 专业计划（Pro） |
| ------------------------------------------------------------ | ----------------------- | --------------- |
| 项目数（Projects）                                           | 200                     | 无限制          |
| 每天创建的部署数（Deployments Created per Day）              | 100                     | 6000            |
| 每次部署创建的无服务器功能数（Serverless Functions Created per Deployment） | 取决于框架*             | 无限制          |
| 每周从CLI创建的部署数（Deployments Created from CLI per Week） | 2000                    | 2000            |
| 每个团队的团队成员数（Team Members per Team）                | -                       | 10              |
| 每个Git仓库连接的Vercel项目数（Vercel Projects Connected per Git Repository） | 3                       | 60              |
| 每次部署创建的路由数（Routes created per Deployment）        | 1024                    | 1024            |
| 每次部署的构建时间（Build Time per Deployment）              | 45分钟                  | 45分钟          |
| 并行构建数（Concurrent Builds）                              | 1                       | 12              |
| 磁盘大小（Disk Size）                                        | 13 GB                   | 13 GB           |
| Cron作业（Cron Jobs）                                        | 2*                      | 40              |
| 包含的使用量                                                 |                         |                 |
| 带宽（Bandwidth）                                            | 100 GB                  | 1 TB            |
| 无服务器功能执行（Serverless Function Execution）            | 100 GB-Hrs              | 1000 GB-Hrs     |
| 边缘功能执行单位（Edge Function Execution Units）            | 500,000                 | 1,000,000       |
| 边缘中间件调用（Edge Middleware Invocations）                | 1,000,000               | 1,000,000       |
| 构建执行（Build Execution）                                  | 100 Hrs                 | 400 Hrs         |
| 图像优化源图像（Image Optimization Source Images）           | 1000图像                | 5000图像        |
| 远程缓存下载（Remote Cache downloads）                       | 10GB                    | 10GB            |
| 远程缓存上传（Remote Cache uploads）                         | 100GB                   | 1TB             |





## 一般限制

|                                                              | Hobby                                                        |              | Pro       | Enterprise |
| :----------------------------------------------------------- | :----------------------------------------------------------- | ------------ | :-------- | :--------- |
| Projects                                                     | 200                                                          | 短期内不会超 | Unlimited | Unlimited  |
| Deployments Created per Day                                  | 100                                                          | 短期内不会超 | 6000      | Custom     |
| Serverless Functions Created per Deployment                  | [Framework-dependent*](https://vercel.com/docs/functions/serverless-functions/runtimes#functions-created-per-deployment) |              | ∞         | ∞          |
| [Proxied Request Timeout ](https://vercel.com/docs/limits/overview#proxied-request-timeout)(Seconds) | 30                                                           |              | 30        | 30         |
| Deployments Created from CLI per Week                        | 2000                                                         | 一样         | 2000      | Custom     |
| [Team Members](https://vercel.com/docs/accounts/team-members-and-roles) per Team | -                                                            | 一样         | 10        | Custom     |
| [Vercel Projects Connected per Git Repository](https://vercel.com/docs/limits/overview#connecting-a-project-to-a-git-repository) | 3                                                            |              | 60        | Custom     |
| [Routes created per Deployment](https://vercel.com/docs/limits/overview#routes-created-per-deployment) | 1024                                                         | 一样         | 1024      | Custom     |
| [Build Time per Deployment](https://vercel.com/docs/limits/overview#build-time-per-deployment)(Minutes) | 45                                                           | 一样         | 45        | 45         |
| [Concurrent Builds](https://vercel.com/docs/deployments/concurrent-builds) | 1                                                            | 这个可能影响 | 12        | Custom     |
| Disk Size (GB)                                               | 13                                                           |              | 13        | 13         |
| Cron Jobs                                                    | [2*](https://vercel.com/docs/cron-jobs/usage-and-pricing)    | 这个可能影响 | 40        | 100        |

## 

## 流量

|                                                              | Hobby       |            | Pro         |
| :----------------------------------------------------------- | :---------- | ---------- | :---------- |
| Bandwidth                                                    | 100 GB      | 流量比较少 | 1 TB        |
| Serverless Function Execution                                | 100 GB-Hrs  | 10倍差距   | 1000 GB-Hrs |
| Edge Function Execution Units                                | 500,000     | 2倍差距    | 1,000,000   |
| Edge Middleware Invocations                                  | 1,000,000   | 一样       | 1,000,000   |
| Build Execution                                              | 100 Hrs     | 4倍        | 400 Hrs     |
| [Image Optimization Source Images](https://vercel.com/docs/image-optimization#source-images) | 1000 Images | 5倍        | 5000 Images |
| [Remote Cache downloads](https://vercel.com/docs/monorepos/remote-caching) | 10GB        | 一样       | 10GB        |
| [Remote Cache uploads](https://vercel.com/docs/monorepos/remote-caching) | 100GB       | 10倍       | 1TB         |



## 日志

每个日志4K。

| Plan       | Retention time                 | Log entries                       |
| :--------- | :----------------------------- | :-------------------------------- |
| Hobby      | 1 hour of logs：只保留一个小时 | Up to 4000 rows of log data       |
| Pro        | 1 day of logs                  | Up to 100,000 rows of log data    |
| Enterprise | 3 days of logs                 | Up to 60 million rows of log data |



## 团队

Hobby只能创建个人项目。

如果要创建团队，只能切换到Pro计划。

<img src="https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240108230735752.png" alt="image-20240108230735752" style="zoom: 33%;" />

Vercel does not support connecting your Personal Account's Projects to Git repositories owned by Git organizations. 



# 基础设施

# Vercel基础设施架构

![img](https://vercel.com/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fv1689795055%2Fdocs-assets%2Fstatic%2Fdocs%2Fconcepts%2Ffunctions%2Fedge-functions-light.png&w=3840&q=75&dpl=dpl_AX4JTQxAUw2CkmFmkXL8unHs8AzT)



## Edge Network

### Regions

| Region Code  | Region Name    | Reference Location    |
| ------------ | -------------- | --------------------- |
| cle1         | us-east-2      | Cleveland, USA        |
| hkg1         | ap-east-1      | Hong Kong             |
| hnd1         | ap-northeast-1 | Tokyo, Japan          |
| iad1(最重要) | us-east-1      | Washington, D.C., USA |
| sfo1         | us-west-1      | San Francisco, USA    |
| sin1         | ap-southeast-1 | Singapore             |



## Serverless Functions

默认情况下，Serverless Functions 在美国华盛顿特区 （ `iad1` ） 执行。



### 查看日志

<img src="https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240108234351555.png" alt="image-20240108234351555" style="zoom: 25%;" />



<img src="https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240108234526225.png" alt="image-20240108234526225" style="zoom: 33%;" />



### 占用的硬件资源

如果配置了 1,769 MB 内存，则无服务器函数将具有相当于一个 vCPU 的内存。

### 内存

- Hobby：最大1G内存
- Pro：最大3G内存

调整内存大小 ：

> vercel.json

```json
{
  "functions": {
    "api/test.js": {
      "memory": 3008
    },
    "api/*.js": {
      "memory": 3008,
      "maxDuration": 30
    }
  }
}
```



## Edge Functions

### Quick Start

1. 创建next.js项目
2. 创建API接口

```ts
// app/api/edge/route.ts

import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';
 
export const runtime = 'edge'; // 'nodejs' is the default
 
export function GET(request: NextRequest) {
  return NextResponse.json(
    {
      body: request.body,
      query: request.nextUrl.search,
      cookies: request.cookies.getAll(),
    },
    {
      status: 200,
    },
  );
}
```

在浏览器访问

```bash
https://vercel-edge-function-two.vercel.app/api/edge?key=123
```

响应

```json
{"body":null,"query":"?key=123","cookies":[]}
```

查看日志

![image-20240109085934289](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240109085934289.png)



[教程](https://vercel.com/docs/functions/edge-functions/quickstart)

[源码](https://github.com/yhb-tutorial/vercel-edge-function)





## Edge Middleware



## Image Optimization

## Incremental Static Regeneration

## Edge Cache

## Data Cache

## Cron Jobs





