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
 
export const runtime = 'edge'; // 关键点，默认是node.js
 
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

位置

处理请求之前执行的代码

本质

运行在Edge Runtime，本质上与Edge Functions一样。

![Edge Middleware location within Vercel infrastructure.](https://vercel.com/_next/image?url=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Fv1689795055%2Fdocs-assets%2Fstatic%2Fdocs%2Fconcepts%2Ffunctions%2Fedge-middleware-light.png&w=3840&q=75&dpl=dpl_2umL2AnS38YL619aBdkbfBU7b3WX)

作用

返回响应之前执行自定义逻辑、重写、重定向、添加标头等。

### 创建Edge Middleware

创建Next.js项目

创建 `middleware.ts` ，放在与 `app` 目录处于同一级。



## Image Optimization

N/A



## Incremental Static Regeneration

1. **传统静态网站生成的限制**：
   - 传统的SSG在构建时会生成网站的所有页面，这对于大型网站来说可能非常耗时。
   - 每次内容更新都需要重新构建整个站点，这可能导致长时间的部署和潜在的服务中断。
2. **ISR的工作原理**：
   - 使用ISR时，你只需在构建时生成一部分页面。
   - 当用户访问尚未生成的页面时，Vercel会在后台生成这些页面，并将它们作为静态内容提供给用户。
   - 一旦页面被生成，它就会被缓存并用于后续的请求，直到下一次预定的再生（regeneration）。
3. **再生策略**：
   - 你可以设置一个时间间隔，告诉Vercel多久重新生成页面。这意味着内容可以定期更新，而不需要手动触发整个站点的重新构建。
   - 这种策略非常适合内容经常变化的网站，如新闻网站或博客。
4. **好处**：
   - **性能优化**：ISR可以显著提高大型网站的构建速度。
     - **实时内容更新**：网站内容可以更频繁且自动地更新，而无需完整的站点重建。
   - **缩短部署时间**：只需生成一部分页面，减少了部署的时间和资源消耗。

### Quick Start

> app/blog-posts/page.tsx

```tsx
interface Post {
    title: string;
    id: number;
}

export default async function Page() {
    const res = await fetch('https://api.vercel.app/blog', {
        next: { revalidate: 10 },
    });
    const posts = (await res.json()) as Post[];
    return (
        <ul>
            {posts.map((post: Post) => {
                return <li key={post.id}>{post.title}</li>;
            })}
        </ul>
    );
}
```

[参考](https://vercel.com/docs/incremental-static-regeneration)



## Edge Cache

原理：通过HTTP头部来控制客户端和CDN服务器的缓存行为。

> HTTP标准的Cache-Control见[这里](https://blog.ververv.com/p/http/#Header)

```ts
// app/api/cache-control-headers/route.ts

export async function GET() {
  return new Response('Cache Control example', {
    status: 200,
    headers: {
      'Cache-Control': 'max-age=10', // 浏览器只缓存10秒
      'CDN-Cache-Control': 'max-age=30', // 中间的CDN厂商只缓存60秒
      'Vercel-CDN-Cache-Control': 'max-age=30', // Vercel的CDN服务器缓存1小时
    },
  });
}
```

在Log中，真正执行/api/cache-control-headers的两次间隔是30秒，说明在Vercel CDN Network中返回了缓存数据。

![image-20240109162849732](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240109162849732.png)



**默认的配置**

 `cache-control: public, max-age=0, must-revalidate` 

边缘网络和浏览器不缓存。

**建议的配置**

 `max-age=0, s-maxage=86400` 

此配置告诉浏览器不要缓存，允许 Vercel 的边缘网络缓存响应并在部署更新时使它们失效。

[Vercel的Cache-Control的定义](https://vercel.com/docs/edge-network/headers#cache-control-header)

[CloudFlare也有类似的Header](https://developers.cloudflare.com/cache/concepts/cdn-cache-control/)







## Data Cache

## Cron Jobs





