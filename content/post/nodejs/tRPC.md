---
title: "tRPC 入门"
description: 
date: 2023-12-25T16:15:41+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# RPC

> Remote Procedure Call

不用关注传输层（HTTP+JSON）的实现，客户端像函数一样直接调用。

```typescript
// HTTP/REST
const res = await fetch('/api/users/1');
const user = await res.json();

// RPC
const user = await api.users.getById({ id: 1 });
```



# tRPC

> tRPC是RPC的其中一种实现



# 概念



| 术语                                                         | 说明                                |
| ------------------------------------------------------------ | ----------------------------------- |
| [**Procedure**](https://trpc.io/docs/server/procedures)      | API EndPoint                        |
| - **Query**                                                  | 类似HTTP GET                        |
| - **Mutation**                                               | 类似HTTP POST/UPDATE/DELETE         |
| - [**Subscription**](https://trpc.io/docs/subscriptions)     | 类似Websocket                       |
| [**Router**](https://trpc.io/docs/server/routers)            | Procedure的集合                     |
| [**Context**](https://trpc.io/docs/server/context)           | 多个Procedure共享的变量             |
| [**Middleware**](https://trpc.io/docs/server/middlewares)    | 调用Procedure前后时，可以使用的钩子 |
| [**Validation **](https://trpc.io/docs/server/procedures#input-validation) |                                     |



# 最小的实例



## 服务端

`server/trpc.ts`

```typescript
import { initTRPC } from '@trpc/server';
 
/**
 * Initialization of tRPC backend
 * Should be done only once per backend!
 */
const t = initTRPC.create();
 
/**
 * Export reusable router and procedure helpers
 * that can be used throughout the router
 */
export const router = t.router;
export const publicProcedure = t.procedure;
```



**定义Procedure**

`server/index.ts`

```typescript
import { db } from './db';
import { publicProcedure, router } from './trpc';
 
const appRouter = router({
  userList: publicProcedure
    .query(async () => {
      // Retrieve users from a datasource, this is an imaginary database
      const users = await db.user.findMany();
             
      return users;
    }),
});
```



**监听API**

server/index.ts

```typescript
import { createHTTPServer } from '@trpc/server/adapters/standalone';
 
const appRouter = router({
  // ...
});
 
const server = createHTTPServer({
  router: appRouter,
});
 
server.listen(3000);
```



## 客户端



**配置客户端**

- 源码级别，引用服务端Procedure的接口定义。
- 配置服务端的host地址。

```typescript
import { createTRPCProxyClient, httpBatchLink } from '@trpc/client';
import type { AppRouter } from './server';
//     👆 **type-only** import
 
// Pass AppRouter as generic here. 👇 This lets the `trpc` object know
// what procedures are available on the server and their input/output types.
const trpc = createTRPCProxyClient<AppRouter>({
  links: [
    httpBatchLink({
      url: 'http://localhost:3000',
    }),
  ],
});
```



**调用Procedure**

```typescript
// Inferred types
const user = await trpc.userById.query('1');
```



[参考](https://trpc.io/docs/quickstart)



# 集成到React

基于React Query。

> 建议先了解[React Query的基本使用](https://blog.ververv.com/p/react-query/)



## 客户端

**导入AppRouter，创建tRPC hooks**

utils/trpc.ts

```typescript
import { createTRPCReact } from '@trpc/react-query';
import type { AppRouter } from '../server/router';
 
export const trpc = createTRPCReact<AppRouter>();
```



**添加tRPC providers**

类似React Query，将整个组件包起来。

创建`QueryClient`和`trpc.createClient`，传入给Provider。

App.tsx

```react
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { httpBatchLink } from '@trpc/client';
import React, { useState } from 'react';
import { trpc } from './utils/trpc';

export function App() {
  const [queryClient] = useState(() => new QueryClient());
  const [trpcClient] = useState(() =>
    trpc.createClient({
      links: [
        httpBatchLink({
          url: 'http://localhost:3000/trpc',

          // You can pass any HTTP headers you wish here
          async headers() {
            return {
              authorization: getAuthCookie(),
            };
          },
        }),
      ],
    }),
  );

  return (
    <trpc.Provider client={trpcClient} queryClient={queryClient}>
      <QueryClientProvider client={queryClient}>
        {/* Your app here */}
      </QueryClientProvider>
    </trpc.Provider>
  );
}
```

**调用Procedure**

pages/IndexPage.tsx

```react
import { trpc } from '../utils/trpc';
 
export default function IndexPage() {
  const userQuery = trpc.getUser.useQuery({ id: 'id_bilbo' });
  const userCreator = trpc.createUser.useMutation();
 
  return (
    <div>
      <p>{userQuery.data?.name}</p>
 
      <button onClick={() => userCreator.mutate({ name: 'Frodo' })}>
        Create Frodo
      </button>
    </div>
  );
}
```



[参考](https://trpc.io/docs/client/react/setup)



# Next.js



参考这篇[文章](https://www.wilchow.com/blog/get-end-to-end-typesafe-apis-with-trpc-and-nextjs-app-router)，写得非常清晰，也附带[示例代码](https://github.com/javajunky/nextjs-trpc-demo)。



# 标准的tRPC代码

```ts
import { initTRPC } from '@trpc/server';
 
// You can use any variable name you like.
// We use t to keep things simple.
const t = initTRPC.create();
 
export const router = t.router;
export const publicProcedure = t.procedure;
```

# 定义apiRouter

```ts
import { publicProcedure, router } from './trpc';
 
const appRouter = router({
  greeting: publicProcedure.query(() => 'hello tRPC v10!'),
});
 
// Export only the type of a router!
// This prevents us from importing server code on the client.
export type AppRouter = typeof appRouter;
```



# Context

不同的procedure共享的数据

> 例如可以放database connections或authentication



```ts
import { initTRPC } from '@trpc/server';
import type { CreateNextContextOptions } from '@trpc/server/adapters/next';
import { getSession } from 'next-auth/react';

// 1、自定义一个Context，返回session
export const createContext = async (opts: CreateNextContextOptions) => {
  const session = await getSession({ req: opts.req });
 
  return {
    session,
  };
};
 
// 2、初始化tRPC时，传入自定义的Context
const t1 = initTRPC.context<typeof createContext>().create();
// 3、在proceures中就可以访问Context中的session                          
t1.procedure.use(({ ctx }) => { ... });
// (parameter) ctx: {
//    session: Session | null;
// }

// 2、初始化tRPC时，传入自定义的Context                             
type Context = Awaited<ReturnType<typeof createContext>>;
const t2 = initTRPC.context<Context>().create();
// 3、在proceures中就可以访问Context中的session
t2.procedure.use(({ ctx }) => { ... });
// (parameter) ctx: {
//    session: Session | null;
// }
```

[参考](https://trpc.io/docs/server/context)



# Middleware

定义一个middleware

在procedure中使用middleware

> t.procedure.use()

使用：Auth或Log



[Middleware官方教程](https://trpc.io/docs/server/middlewares#extending-middlewares)





