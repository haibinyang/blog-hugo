---
title: "Zod"
description: 
date: 2023-12-27T15:33:11+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# Type inference

[官方说明](https://zod.dev/?id=type-inference)

 `z.infer<typeof mySchema>` 

```ts
const A = z.string();
type A = z.infer<typeof A>; // string

const u: A = 12; // TypeError
const u: A = "asdf"; // compiles
```



