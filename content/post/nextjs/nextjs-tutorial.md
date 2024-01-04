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

