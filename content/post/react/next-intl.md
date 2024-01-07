---
title: "Next.js国际化和next-intl"
description: 
date: 2024-01-04T13:32:18+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---

# 前言

# i18n

> `internationalization`中间有18个字母，缩写成`i18n`



## BCP-47标准

用于标识人类语言。

> 这个名字代表“最佳实践建议书47”（Best Current Practice 47），是互联网工程任务组（IETF）的一部分。



[BCP 47](https://www.rfc-editor.org/rfc/bcp/bcp47.txt)标签的结构通常包括：

1. **语言代码**：基于ISO 639-1（两字母代码）或ISO 639-2（三字母代码）的标准，用于表示特定语言。例如，英语是"en"，中文是"zh"。
2. **地区代码**：基于ISO 3166-1 alpha-2标准的两字母国家或地区代码。例如，美国是"US"，中国是"CN"。
3. **脚本代码**：基于ISO 15924标准，用四个字母表示特定的书写系统。例如，简体中文的脚本代码是"Hans"，繁体中文是"Hant"。一个语言有多种写法。

> 例如，"zh-Hans-CN"代表中国的简体中文，其中"zh"是语言代码，"Hans"表示简体中文脚本，"CN"是国家代码。



## ICU

> International Components for Unicode
>
> [ICU](https://unicode-org.github.io/icu/userguide/format_parse/messages/)是一个成熟、广泛使用的跨平台Unicode库，主要用于C、C++和Java程序。ICU提供了一系列与Unicode相关的功能，包括文本处理、字符集转换、日期和时间格式化、本地化支持等。







# Next.js国际化

待定



# next-intl

安装库

```bash
 npm install next-intl
```

文件目录

```
├── messages (1)
│   ├── en.json
│   └── ...
├── next.config.js (2)
└── src
    ├── i18n.ts (3)
    ├── middleware.ts (4)
    └── app
        └── [locale]
            ├── layout.tsx (5)
            └── page.tsx (6)
```

## （1）`messages/en.json`

定义strings文件

> messages/en.json

```json
{
  "Index": {
    "title": "Hello world!"
  }
}
```

## （2）`src/i18n.ts`

新增next-intl的配置文件

- 指定messages的路径

```ts
import {notFound} from "next/navigation";
import {getRequestConfig} from 'next-intl/server';
 
// Can be imported from a shared config
const locales = ['en', 'de'];
 
export default getRequestConfig(async ({locale}) => {
  // Validate that the incoming `locale` parameter is valid
  if (!locales.includes(locale as any)) notFound();
 
  return {
    messages: (await import(`../messages/${locale}.json`)).default
  };
});
```



## （3）`next.config.js`

指定`i18n.ts`配置文件的路径

```ts
const withNextIntl = require('next-intl/plugin')("./src/i18n.ts");// 指定`i18n.ts`配置文件的路径

/** @type {import('next').NextConfig} */
const nextConfig = {}

module.exports = withNextIntl(nextConfig);
```

## （4）`middleware.ts`

- 指定哪些路径需要使用middleware来处理
- Middleware使用next-intl返回的middleware

```ts
import createMiddleware from 'next-intl/middleware';

// next-intl来实现middleware
export default createMiddleware({
    // A list of all locales that are supported
    locales: ['en', 'de'],

    // Used when no locale matches
    defaultLocale: 'en'
});

// 指定哪些路径需要使用middleware来处理
export const config = {
    // Match only internationalized pathnames
    matcher: ['/', '/(de|en)/:path*']
};
```

> 在middleware中有一个配置项：localePrefix

```ts
import createMiddleware from 'next-intl/middleware';
 
export default createMiddleware({
  // ... other config
 
  localePrefix: 'always' // 默认值
  localePrefix: 'as-needed' //  default locale不会重定向到/en/about
});
```





## （5）`app/[locale]/layout.tsx`

将locale变量放到html中的lang属性

```tsx
export default function LocaleLayout({ children, params: { locale } }) {
    return (
        <html lang={locale}>
            <body>{children}</body>
        </html>
    );
}
```

## （6）使用

`app/[locale]/page.tsx`

```tsx
import {useTranslations} from 'next-intl';
 
export default function Index() {
  const t = useTranslations('Index');
  
  return <h1>{t('title')}</h1>;
}
```



[教程](https://next-intl-docs.vercel.app/docs/getting-started/app-router)

## 范例代码

见[Github](https://github.com/yhb-tutorial/next-intl)



# 两个重要函数

## getTranslations

```ts
// 只传namespace
getTranslations(namespace?: NestedKey)

// 传locale和namespace
getTranslations(opts?: {locale: string; namespace?: NestedKey;})
```



## useTranslations

待定



# 使用场景

## Server & Client Components

### Server Components

#### Async components

```tsx
import {getTranslations} from 'next-intl/server'; // 1. Server版本
 
export default async function ProfilePage() {
  const user = await fetchUser();
  const t = await getTranslations('ProfilePage'); // 2. getTranslations()
 
  return (
    <PageLayout title={t('title', {username: user.name})}>
      <UserDetails user={user} />
    </PageLayout>
  );
}
```

> 其它方法
>
> ```ts
> const t = await getTranslations('ProfilePage');
> const format = await getFormatter();
> const now = await getNow();
> const timeZone = await getTimeZone();
> const messages = await getMessages();
> const locale = await getLocale();
> ```



#### Non-async components

next-intl根据当前组件情况，自动渲染成server或client组件。

```tsx
import {useTranslations} from 'next-intl';
 
export default function UserDetails({user}) {
  const t = useTranslations('UserProfile');
 
  return (
    <section>
      <h2>{t('title')}</h2>
    </section>
  );
}
```



> 如果您实现的组件符合共享组件的条件，将它们实现为Non-async components可能会很有益。这样可以在服务器或客户端环境中使用这些组件，使它们非常灵活。即使您不打算将某个特定组件运行在客户端，这种兼容性仍然有助于简化测试。



### Client Components

#### 方案一

在Server Components渲染，然后传给Client Components

> [locale]/faq/page.tsx

```tsx
import {useTranslations} from 'next-intl';
import Expandable from './Expandable';
 
export default function FAQEntry() {
  // 1、Call `useTranslations` in a Server Component ...
  const t = useTranslations('FAQEntry');
 
  // 2. pass translated content to a Client Component
  return (
    <Expandable title={t('title')}>
			...
    </Expandable>
  );
}
```

> Expandable.tsx

```tsx
'use client';
 
function Expandable({title, children}) {
  ...
  return (
    <div>
      <button onClick={onToggle}>{title}</button>
    </div>
  );
}
```



#### 方案四：提供所有文案

如果您正在构建一个高度动态的应用程序，其中大多数组件使用React的交互功能，您可能更倾向于让所有消息都能被客户端组件使用。

> app/[locale]/layout.tsx

```tsx
import {NextIntlClientProvider, useMessages} from 'next-intl';
 
export default function LocaleLayout({children, params: {locale}}) {
  // ...
  // Receive messages provided in `i18n.ts`
  const messages = useMessages();
 
  return (
    <html lang={locale}>
      <body>
        <NextIntlClientProvider locale={locale} messages={messages}>
          {children}
        </NextIntlClientProvider>
      </body>
    </html>
  );
}
```

[参考](https://next-intl-docs.vercel.app/docs/environments/server-client-components)



## Metadata

```tsx
// app/[locale]/layout.tsx

import {getTranslations} from 'next-intl/server';
 
export async function generateMetadata({params: {locale}}) {
  // 从generateMetadata中获取参数locale
  const t = await getTranslations({locale, namespace: 'Metadata'});
 
  return {
    title: t('title')
  };
}
```



## Route Handlers

```tsx
// app/api/hello/route.tsx

import {NextResponse} from 'next/server';
import {getTranslations} from 'next-intl/server';
 
export async function GET(request) {
  // Example: Receive the `locale` via a search param
  const {searchParams} = new URL(request.url);
  const locale = searchParams.get('locale');
 
  const t = await getTranslations({locale, namespace: 'Hello'});
  return NextResponse.json({title: t('title')});
}
```



## error files

1. `not-found.js`
2. `error.js`



有时间再来学习：[教程](https://next-intl-docs.vercel.app/docs/environments/error-files#errorjs)



# 支持的文案格式

## 结构化的文案



```json
{
  "About": {
    "title": "About us"
  }
}
```

使用

```tsx
import {useTranslations} from 'next-intl';
 
function About() {
  const t = useTranslations('About');
  
  return <h1>{t('title')}</h1>;
}
```

或者

```ts
const t = useTranslations();
 
t('About.title');
```



## ICU格式的文案



### 内嵌变量

文案：en.json

```json
{
  "title": "欢迎, {username}!"
}
```

使用

```ts
const t = await getTranslations('ProfilePage');

t('title', {username: user.name})
```

### 复数

> en.json

```json
"message": "You have {count, plural, =0 {no followers yet} =1 {one follower} other {# followers}}."
```

使用

```tsx
t('message', {count: 3580}); // "You have 3,580 followers."
```

### 多选一

> en.json

```tsx
"message": "{gender, select, female {She} male {He} other {They}} is online."
```

```ts
t('message', {gender: 'female'}); // "She is online."
```

## Rich Text

> en.js

```json
{
  "message": "Please refer to <guidelines>the guidelines</guidelines>."
}
```

```TSX
// Returns `<>Please refer to <a href="/guidelines">the guidelines</a>.</>`
t.rich('message', {
  guidelines: (chunks) => <a href="/guidelines">{chunks}</a>
});
```



## 格式化普通数字

```ts
import {useFormatter} from 'next-intl';
 
function Component() {
  const format = useFormatter();
 
  // Renders "$499.90"
  format.number(499.9, {style: 'currency', currency: 'USD'});
  
  format.number(3500, {style: 'decimal'}); // '3,500'
  format.number(3500, {style: 'percent'}); // '350,000%'
  
  format.number(50 {style: "unit", nit: "kilometer-per-hour",}); // 50 km/h
}
```

[style](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl/NumberFormat/NumberFormat#style)的可选值

| English Term | Chinese Translation | 可选值                                                       |
| ------------ | ------------------- | ------------------------------------------------------------ |
| decimal      | 十进制              |                                                              |
| currency     | 货币                |                                                              |
| percent      | 百分比              |                                                              |
| unit         | 单位                | [链接](https://github.com/unicode-org/cldr/blob/main/common/validity/unit.xml) |

## 内嵌数字到ICU文案

```json
{
  "basic": "Basic formatting: {value, number}",
  "percentage": "Displayed as a percentage: {value, number, percent}",
  "custom": "At most 2 fraction digits: {value, number, ::.##}"
}
```

## 格式化时间和日期



```tsx
import {useFormatter} from 'next-intl';
 
function Component() {
  const format = useFormatter();
  const dateTime = new Date('2020-11-20T10:36:01.516Z');
 
  // Renders "Nov 20, 2020"
  format.dateTime(dateTime, {
    year: 'numeric',
    month: 'short',
    day: 'numeric'
  });
 
  // Renders "11:36 AM"
  format.dateTime(dateTime, {hour: 'numeric', minute: 'numeric'});
}
```

详细的举例见[这里](https://www.intl-explorer.com/DateTimeFormat?locale=zh-Hans-CN)

## 内嵌日期/时间到ICU文案

```json
{
  "ordered": "Ordered on {orderDate, date, medium}"
}
```

```json
{
  "ordered": "Ordered on {orderDate, date, ::yyyyMd}"
}
```

[参考](https://next-intl-docs.vercel.app/docs/usage/dates-times#dates-and-times-within-messages)



# VSCode插件：i18n-ally

[安装地址](https://marketplace.visualstudio.com/items?itemName=lokalise.i18n-ally)

配置语言文件的地址和使用的框架。

![image-20240104142115694](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240104142115694.png)
