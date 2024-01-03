---
title: "Lucia Auth"
description: 
date: 2023-12-27T22:05:24+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# 标准

安装

```bash
npm i lucia
```

初始化

```ts
// lucia.ts
import { lucia } from "lucia";

// expect error (see next section)
export const auth = lucia({
	env: "DEV" // "PROD" if deployed to HTTPS
});

export type Auth = typeof auth;
```

Middleware

```ts
import { lucia } from "lucia";
import { node } from "lucia/middleware";

export const auth = lucia({
	env: "DEV", // "PROD" if deployed to HTTPS
	middleware: node()
});
```

配置数据库

```ts
import { lucia } from "lucia";
import { prisma } from "@lucia-auth/adapter-prisma";
import { PrismaClient } from "@prisma/client";

const client = new PrismaClient();

const auth = lucia({
	env: "DEV", // "PROD" if deployed to HTTPS
	adapter: prisma(client)
});
```



# 集成到Next.js

[参考](https://lucia-auth.com/guidebook/sign-in-with-username-and-password/nextjs-app/)

## 配置app.d.ts

```ts
// app.d.ts

/// <reference types="lucia" />
declare namespace Lucia {
	type Auth = import("./lucia.js").Auth;
	type DatabaseUserAttributes = {
		username: string;
	};
	type DatabaseSessionAttributes = {};
}
```

## 配置Lucia

- expires: false：we can’t update the session cookie when validating them.
-  expose the user’s username to the `User` object by defining [`getUserAttributes`](https://lucia-auth.com/basics/configuration#getuserattributes).

```ts
// auth/lucia.ts
import { lucia } from "lucia";
import { nextjs_future } from "lucia/middleware";

export const auth = lucia({
	adapter: ADAPTER,
	env: process.env.NODE_ENV === "development" ? "DEV" : "PROD",
	middleware: nextjs_future(),
	sessionCookie: {
		expires: false
	},

	getUserAttributes: (data) => {
		return {
			username: data.username
		};
	}
});

export type Auth = typeof auth;
```

## 注册

1. 创建用户：`auth.createUser`
2. 创建session：`auth.createSession`
3. 创建authRequest：`auth.handleRequest(req.method, context);`
4. 设置cookie：`authRequest.setSession(session);`



```ts
import { auth } from "../../auth/lucia";
import * as context from "next/headers";
import { NextRequest, NextResponse } from "next/server";

export const POST = async (req: NextRequest) => {
    try {
        const data = await req.json();
        const { email, password, name, role } = data;

        const user = await auth.createUser({
            key: {
                providerId: "email",
                providerUserId: email,
                password,
            },
            attributes: {
                email,
                name,
                role,
            },
        });

        const session = await auth.createSession({
            userId: user.userId,
            attributes: {},
        });

        const authRequest = await auth.handleRequest(req.method, context);
        authRequest.setSession(session);

        return new Response(
            JSON.stringify({
                message: "User created",
            }),
            {
                status: 201,
            }
        );
    } catch (e: any) {
        return NextResponse.json(
            {
                error: "An unknown error occurred",
            },
            {
                status: 500,
            }
        );
    }
};
```



## 登陆

1. 验证帐号密码：auth.useKey()

   > 捕获Error，判断是否密码错误。
   >
   > ```ts
   > if (e instanceof LuciaError &&
   >             (e.message === "AUTH_INVALID_KEY_ID" 
   >              || e.message === "AUTH_INVALID_PASSWORD")
   > )
   > ```

   

2. 创建session：auth.createSession()

3. 创建authRequest：`auth.handleRequest(req.method, context);`

4. 设置cookie：`authRequest.setSession(session);`

```ts
import { auth } from "../../auth/lucia";
import * as context from "next/headers";
import { NextRequest, NextResponse } from "next/server";
import { LuciaError } from "lucia";

export const POST = async (req: NextRequest) => {
    try {
        const data = await req.json();
        const { email, password } = data;

        const key = await auth.useKey("email", email.toLowerCase(), password);
        const session = await auth.createSession({
            userId: key.userId,
            attributes: {},
        });

        const authRequest = auth.handleRequest(req.method, context);
        authRequest.setSession(session);

        return new Response(
            JSON.stringify({
                message: "User logged in",
            }),
            {
                status: 200,
            }
        );
    } catch (e: any) {
        if (e instanceof LuciaError &&
            (e.message === "AUTH_INVALID_KEY_ID" || e.message === "AUTH_INVALID_PASSWORD")
        ) {
            console.error(e.message);
            // user does not exist or invalid password
            return NextResponse.json(
                {
                    error: "Incorrect username or password",
                },
                {
                    status: 400,
                }
            );
        }
        return NextResponse.json(
            {
                error: "An unknown error occurred",
            },
            {
                status: 500,
            }
        );
    }
};
```



登陆请求返回一个`Set-Cookie`HTTP Header

```json
auth_session=uceupxiddx2jwveob1c2fjffgiz1df6rytqu032e; Path=/; Expires=Wed, 01 Jan 2025 07:26:20 GMT; HttpOnly; SameSite=lax
```

> 1. `HttpOnly`：这个属性是一个安全措施，表示该Cookie只能通过HTTP请求被访问，而不能通过客户端脚本（如JavaScript）直接访问。这有助于防止跨站脚本（XSS）攻击。
> 2. `SameSite=lax`：这个属性用于防止跨站请求伪造（CSRF）攻击。`SameSite=lax`意味着Cookie在跨站请求中不会被发送，但在导航到目标网站的顶级请求（如链接点击）时会被发送。这是一个相对宽松的设置，适用于大多数网站。



![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20240102152845.png)

最终存在这里。

![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20240102153705.png)



## 登出

1. 创建authRequest：`auth.handleRequest(req.method, context);`
2. 尝试获取合法的session：`authRequest.validate();`
3. 将session设置为失效：`auth.invalidateSession(session.sessionId);`
4. 删除session cookie：`authRequest.setSession(null);`



```ts
import { auth } from "../../auth/lucia";
import * as context from "next/headers";

import type { NextRequest } from "next/server";

export const POST = async (request: NextRequest) => {
    const authRequest = auth.handleRequest(request.method, context);
    // check if user is authenticated
    const session = await authRequest.validate();
    if (!session) {
        return new Response(null, {
            status: 401,
        });
    }
    // make sure to invalidate the current session!
    await auth.invalidateSession(session.sessionId);
    // delete session cookie
    authRequest.setSession(null);
    return new Response(
        JSON.stringify({
            message: "User logged out",
        }),
        {
            status: 302,
        }
    );
};
```



logout的响应的HTTP Header:

```
Set-Cookie:
auth_session=; Path=/; Expires=Thu, 01 Jan 1970 00:00:00 GMT; HttpOnly; SameSite=lax
```

![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20240102153806.png)



Cookies中的`auth_session`不见了。

![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20240102153904.png)



## 参考代码

见[Github](https://github.com/yhb-tutorial/lucia-auth-next.js)



# 基本

## 数据库表

用户表

| name 名字 | type 类型 | primary key 主键 | description 描述 |
| --------- | --------- | ---------------- | ---------------- |
| id        | `string`  | ✓                | User id 用户 ID  |

会话表

| name 名字      | type 类型                         | primary key 主键 | references 引用 | description 描述                                             |
| -------------- | --------------------------------- | ---------------- | --------------- | ------------------------------------------------------------ |
| id             | `string`                          | ✓                |                 |                                                              |
| user_id        | `string`                          |                  | `user(id)`      |                                                              |
| active_expires | `number` (int8) `number` （int8） |                  |                 | The expiration time (unix) of the session (active) 会话的过期时间 （unix）（活动） |
| idle_expires   | `number` (int8) `number` （int8） |                  |                 | The expiration time (unix) for the idle period 空闲期的过期时间 （unix） |

>  Active sessions are your access tokens, and idle sessions are your refresh tokens.

密钥表

| name 名字       | type 类型       | primary key 主键 | references 引用 | description 描述                                             |
| --------------- | --------------- | ---------------- | --------------- | ------------------------------------------------------------ |
| id              | `string`        | ✓                |                 | Key id in the form of: `${providerId}:${providerUserId}` 密钥 ID，格式为： `${providerId}:${providerUserId}` |
| user_id         | `string`        |                  | `user(id)`      |                                                              |
| hashed_password | `string | null` |                  |                 | Hashed password of the key 密钥的哈希密码                    |

## 用户

创建用户

```ts
import { auth } from "./lucia.js";
import { LuciaError } from "lucia";

try {
	const user = await auth.createUser({
		key: {
			providerId,
			providerUserId,
			password
		},
		attributes: {
			username
		} // expects `Lucia.DatabaseUserAttributes`
	});
} catch (e) {
	if (e instanceof LuciaError && e.message === `AUTH_DUPLICATE_KEY_ID`) {
		// key already exists
	}
	// provided user attributes violates database rules (e.g. unique constraint)
	// or unexpected database errors
}
```

## 密钥

### 验证帐号和密码

```ts
import { auth } from "./lucia.js";

const key = await auth.useKey("email", "user@example.com", "123456");
const user = await auth.getUser(key.userId);
```

### 验证OAuth

```ts
import { auth } from "./lucia.js";

const githubUser = await authenticateWithGithub(); // example - exact API not provided by Lucia
const key = await auth.useKey("github", githubUser.userId);
```

### 修改密码

```ts
import { auth } from "./lucia.js";

try {
	const key = await auth.updateKeyPassword(
		providerId,
		providerUserId,
		newPassword
	);
} catch (e) {
	if (e instanceof LuciaError && e.message === "AUTH_INVALID_KEY_ID") {
		// invalid key
	}
	// unexpected database error
}
```

```ts
await auth.updateKeyPassword("email", "user@example.com", "654321");
```

## 会话

> 会话ID：长度为 40 个字符。

会话 ID 可以存储在 cookie 中，也可以放在token。

### 会话的状态

会话可以处于以下 3 种状态之一：

- 活动的：有效的会话。一段时间后进入“空闲的”。
- 空闲的: 有效的会话，但 Lucia 将重置过期时间。如果不使用会话，则变为“死的”。
- 死的：无效会话。用户必须重新登录。

### 数据表

| name 名字      | type 类型       | description 描述                                   |
| -------------- | --------------- | -------------------------------------------------- |
| id             | `string`        |                                                    |
| user_id        | `string`        |                                                    |
| active_expires | `number` (int8) | The expiration time (unix) of the session (active) |
| idle_expires   | `number` (int8) | The expiration time (unix) for the idle period     |

### 逻辑

通过延长过期时间来重置会话；不会使空闲会话失效并创建新会话。

如果您使用了访问令牌和刷新令牌，则 Lucia 的会话是两者的组合。

 Active sessions are your access tokens, and idle sessions are your refresh tokens.



### 创建会话

```ts
import { auth } from "./lucia.js";
import { LuciaError } from "lucia";

try {
	const session = await auth.createSession({
		userId,
		attributes: {} // expects `Lucia.DatabaseSessionAttributes`
	});
	const sessionCookie = auth.createSessionCookie(session);
	setSessionCookie(session);
} catch (e) {
	if (e instanceof LuciaError && e.message === `AUTH_INVALID_USER_ID`) {
		// 如果用户 ID 无效，则会抛出 AUTH_INVALID_USER_ID .
	}
	// unexpected database errors
}
```

### 使会话失效

```ts
import { auth } from "./lucia.js";

await auth.invalidateSession(sessionId);
```



使所有用户会话失效

```ts
import { auth } from "./lucia.js";

await auth.invalidateAllUserSessions(userId);
```



## Handle requests

读取和解析请求标头、验证会话以及为每个受保护的终结点设置适当的响应标头有点繁琐。

为了解决这个问题，Lucia 提供了一个 `Auth.handleRequest()` 创建新 `AuthRequest` 实例的方法。

这提供了一些方法，可以更轻松地使用会话 Cookie 和持有者令牌。



但是，每个框架和运行时都有自己的传入请求和传出响应的表示形式。Lucia通过中间件解决。



配置Netxt.js

We recommend setting [`sessionCookie.expires`](https://lucia-auth.com/basics/configuration#sessioncookie) configuration to `false` when using this middleware.

```ts
import { node } from "lucia/middleware";

lucia({
	// ...
	middleware: nextjs_future() // pass Web middleware
});

// `Auth.handleRequest()` now accepts `Request`
const authRequest = auth.handleRequest(new Request());
```

```ts
// app/routes.ts
import * as context from "next/headers";

export const POST = async (request: NextRequest) => {
	const authRequest = auth.handleRequest(request.method, context);
	// ...
};
```

```ts
// middleware.ts
export const middleware = async (request: NextRequest) => {
	// `AuthRequest.setSession()` is not supported when only `NextRequest` is passed
	const authRequest = auth.handleRequest(request);
	// ...
	const session = await auth.createSession({
		// ...
	});
	const sessionCookie = auth.createSessionCookie(session);
	const response = new Response(null);
	response.headers.append("Set-Cookie", sessionCookie.serialize());
};
```



## 使用Cookies

### Cookie expiration

- 默认情况下，会话 Cookie 设置为在会话过期时过期。
- 如果在延长会话过期时间后无法始终设置 cookie，则此行为可能更不可取。
- 您可以通过将 configuration 设置为 `sessionCookie.expires` `false` 来将会话 cookie 设置为无限期持续。启用此选项不会更改会话过期时间，而只会更改 cookie。

### 创建Session



### 创建Cookie

您可以使用 创建一个新的 `Cookie` `Auth.createSessionCookie()` ，它接受 `Session` . `Cookie.serialize()` 可用于生成新的 `Set-Cookie` 响应标头值。您还可以访问 Cookie 名称、值和属性（例如 和 `httpOnly` `maxAge` ）。

```ts
import { auth } from "./lucia.js";

...
const sessionCookie = auth.createSessionCookie(session);
setResponseHeaders("Set-Cookie", sessionCookie.serialize());
```



### 验证Cookie

```ts
import { auth } from "./lucia.js";

const authRequest = auth.handleRequest();
const session = await authRequest.validate();
if (session) {
	// valid request
}
```

- [`AuthRequest.validate()`](https://lucia-auth.com/reference/lucia/interfaces/authrequest#validate) validates the request origin and the session cookie stored
- Reset sessions if they’re idle.

#### 缓存

`AuthRequest.validate()` 缓存请求，因此无论您调用多少次，它都只会运行一次。

```ts
await authRequest.validate();
await authRequest.validate(); // uses cache from previous call
```

> 每当调用缓存时 `AuthRequest.setSession()` ，缓存都会失效。



### 删除Cookie

您可以传递 `null` 删除会话 Cookie。

```ts
import { auth } from "./lucia.js";

const authRequest = auth.handleRequest();
authRequest.setSession(null); // delete session cookie
```



### 设置Session

```ts
import { auth } from "./lucia.js";

const authRequest = auth.handleRequest();
authRequest.setSession(null); // delete session cookie
```





[参考](https://lucia-auth.com/basics/sessions/)

# OAuth



