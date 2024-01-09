---
title: "HTTP"
description: 
date: 2024-01-09T15:12:53+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# Header

## Cache-Control

> 在请求和响应中——控制浏览器和共享缓存（例如代理、CDN）中的缓存。

如下的可选值：

| Request 请求     | Response 响应            |                                                              |
| :--------------- | :----------------------- | ------------------------------------------------------------ |
| `max-age`        | `max-age`                | 指定从请求开始的最大新鲜时间（以秒为单位）。超过这个时间，缓存的资源被认为是过时的。 |
| `max-stale`      | -                        |                                                              |
| `min-fresh`      | -                        |                                                              |
| -                | `s-maxage`               | 定义共享缓存（例如，代理服务器或 CDN）中资源的最大缓存时间。<br/>这个指令仅对共享缓存有效，对浏览器这类私有缓存不起作用。<br/>优先级高于 max-age 和客户端的本地缓存设置。 |
| `no-cache`       | `no-cache`               | 每次请求时都必须向服务器验证资源的有效性。即使缓存了资源，浏览器也会向服务器发送一个验证请求。 |
| `no-store`       | `no-store`               | 完全不存储任何关于客户端请求和服务器响应的内容。通常用于敏感数据。 |
| `no-transform`   | `no-transform`           |                                                              |
| `only-if-cached` | -                        |                                                              |
| -                | `must-revalidate`        |                                                              |
| -                | `proxy-revalidate`       | 一旦资源过期（例如超过了 max-age 指定的时间），在使用这个缓存之前，必须向服务器验证资源是否仍然有效。 |
| -                | `must-understand`        |                                                              |
| -                | `private`                | 响应仅为单个用户私有，只有特定用户的浏览器可以存储响应。     |
| -                | `public`                 | 允许响应被任何缓存所存储，包括中介服务器。                   |
| -                | `immutable`              |                                                              |
| -                | `stale-while-revalidate` |                                                              |
| `stale-if-error` | `stale-if-error`         |                                                              |



缓存分：`private`/`public`/共享

处理缓存的逻辑：

服务器：`max-age`、`no-cache`、`no-store`

代理服务器或CDN服务器：`s-maxage`（针对共享缓存）

[教程](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control)