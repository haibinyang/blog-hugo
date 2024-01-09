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





# Route Segment Config

The Route Segment options allows you to configure the behavior of a [Page](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts), [Layout](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts), or [Route Handler](https://nextjs.org/docs/app/building-your-application/routing/route-handlers) by directly exporting the following variables:

> layout.tsx | page.tsx | route.ts

```ts
export const dynamic = 'auto'
export const dynamicParams = true
export const revalidate = false
export const fetchCache = 'auto'
export const runtime = 'nodejs' // 'edge' | 'nodejs'
export const preferredRegion = 'auto'
export const maxDuration = 5
 
export default function MyComponent() {}
```



| Option                                                       | Type                                                         | Default                    |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------- |
| [`dynamic`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#dynamic) | `'auto' | 'force-dynamic' | 'error' | 'force-static'`        | `'auto'`                   |
| [`dynamicParams`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#dynamicparams) | `boolean`                                                    | `true`                     |
| [`revalidate`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#revalidate) | `false | 0 | number`                                         | `false`                    |
| [`fetchCache`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#fetchcache) | `'auto' | 'default-cache' | 'only-cache' | 'force-cache' | 'force-no-store' | 'default-no-store' | 'only-no-store'` | `'auto'`                   |
| [`runtime`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#runtime) | `'nodejs' | 'edge'`                                          | `'nodejs'`                 |
| [`preferredRegion`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#preferredregion) | `'auto' | 'global' | 'home' | string | string[]`             | `'auto'`                   |
| [`maxDuration`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#maxduration) | `number`                                                     | Set by deployment platform |



### [`dynamic`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#dynamic)

Change the dynamic behavior of a layout or page to fully static or fully dynamic.

- **`'force-dynamic'`**: Force [dynamic rendering](https://nextjs.org/docs/app/building-your-application/rendering/server-components#dynamic-rendering), which will result in routes being rendered for each user at request time. 

- **`'force-static'`**: Force static rendering and cache the data of a layout or page by forcing [`cookies()`](https://nextjs.org/docs/app/api-reference/functions/cookies), [`headers()`](https://nextjs.org/docs/app/api-reference/functions/headers) and [`useSearchParams()`](https://nextjs.org/docs/app/api-reference/functions/use-search-params) to return empty values.

### [`revalidate`](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config#revalidate)

Set the default revalidation time for a layout or page. This option does not override the `revalidate` value set by individual `fetch` requests.

```ts
export const revalidate = false
// false | 0 | number
```

**`false`**: (default) The default heuristic to cache any `fetch` requests that set their `cache` option to `'force-cache'` or are discovered before a [dynamic function](https://nextjs.org/docs/app/building-your-application/rendering/server-components#server-rendering-strategies#dynamic-functions) is used. 

**`0`**: Ensure a layout or page is always [dynamically rendered](https://nextjs.org/docs/app/building-your-application/rendering/server-components#dynamic-rendering) even if no dynamic functions or uncached data fetches are discovered. 



[参考](https://nextjs.org/docs/app/api-reference/file-conventions/route-segment-config)
