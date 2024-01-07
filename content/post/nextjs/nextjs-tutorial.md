---
title: "Nextjs Tutorial"
description: 
date: 2023-12-03T06:28:27+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---





# Next.js CLI

[官方文档](https://nextjs.org/docs/pages/api-reference/next-cli)



# 动态路由

**Dynamic Segment**

`[segmentName]`

例如 `[id]` or `[slug]`.



## 举例

> pages/blog/[slug].js

```ts
import { useRouter } from 'next/router'
 
export default function Page() {
  const router = useRouter()
  return <p>Post: {router.query.slug}</p>
}
```

| Route                  | Example URL | `params`        |
| ---------------------- | ----------- | --------------- |
| `pages/blog/[slug].js` | `/blog/a`   | `{ slug: 'a' }` |
| `pages/blog/[slug].js` | `/blog/b`   | `{ slug: 'b' }` |
| `pages/blog/[slug].js` | `/blog/c`   | `{ slug: 'c' }` |

## Catch-all Segments

`[...segmentName]`

| Route                     | Example URL   | `params`                    |
| ------------------------- | ------------- | --------------------------- |
| `pages/shop/[...slug].js` | `/shop/a`     | `{ slug: ['a'] }`           |
| `pages/shop/[...slug].js` | `/shop/a/b`   | `{ slug: ['a', 'b'] }`      |
| `pages/shop/[...slug].js` | `/shop/a/b/c` | `{ slug: ['a', 'b', 'c'] }` |



## 可选的Catch-all Segments

`[[...segmentName]]`

| Route                       | Example URL            | `params`                    |
| --------------------------- | ---------------------- | --------------------------- |
| `pages/shop/[[...slug]].js` | `/shop`<br/>区别在这里 | `{ slug: [] }`              |
| `pages/shop/[[...slug]].js` | `/shop/a`              | `{ slug: ['a'] }`           |
| `pages/shop/[[...slug]].js` | `/shop/a/b`            | `{ slug: ['a', 'b'] }`      |
| `pages/shop/[[...slug]].js` | `/shop/a/b/c`          | `{ slug: ['a', 'b', 'c'] }` |



# Middleware

## 作用

中间件允许您在请求完成之前运行代码。然后，根据传入的请求，您可以通过重写、重定向、修改请求或响应标头或直接响应来修改响应。

## 定义一个Middleware

- 放在项目根目录中：例如，与 `pages` 或 `app` 位于同一级别，或者位于内部 `src` （如果适用）。
- 文件名是 `middleware.ts` 

## 范例



```ts
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'
 
// This function can be marked `async` if using `await` inside
export function middleware(request: NextRequest) {
  return NextResponse.redirect(new URL('/home', request.url))
}
 
// See "Matching Paths" below to learn more
export const config = {
  matcher: '/about/:path*',
}
```



[参考](https://nextjs.org/docs/app/building-your-application/routing/middleware)



# Metadata

**静态**

> layout.tsx | page.tsx

```ts
import type { Metadata } from 'next'
 
export const metadata: Metadata = {
  title: '...',
  description: '...',
}
 
export default function Page() {}
```

**动态**

使用 `generateMetadata` 函数

> 是一个async函数

```tsx
// app/products/[id]/page.tsx

export async function generateMetadata() {
  return {
    title: "blog.title",
  };
}
 
export default function Page({ params, searchParams }: Props) {}
```

[参考](https://nextjs.org/docs/app/building-your-application/optimizing/metadata)
