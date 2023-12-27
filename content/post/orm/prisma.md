---
title: "Prisma入门"
description: 
date: 2023-12-27T09:27:42+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# Prisma

- ORM框架
- 基于JavaScript/TypeScript语言



# 快速入门

参考[官方教程](https://www.prisma.io/docs/getting-started/quickstart)，使用一个标准的Typescript项目来访问SQLite。

> 可以将dev.db修改成dev.sqlite
>
> 安装VSCode插件（SQLite Viewer），这样打开dev.sqlite文件，可以直接操作SQLite数据。



## 基本流程

1. 安装prisma库

 ```bash
   npm install prisma --save-dev
 ```

2. 初始化Prisma

   ```bash
   npx prisma init --datasource-provider sqlite
   ```

3. 定义Prisma Schema

```typescript
   model User {
     id    Int     @id @default(autoincrement())
     email String  @unique
     name  String?
   }
```

3. 创建迁移

```bash
   npx prisma migrate dev --name init
```

> 生成PrismaClient

4. 调用

```typescript
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient()
const users = await prisma.user.findMany()
```



## Prisma的命令



| 命令         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| init         | `npx prisma init --datasource-provider sqlite`               |
| format       | 格式化Prisma  schema文件                                     |
| generate     | 生成Prisma Client                                            |
| db push      | 将 Prisma schema文件推送到数据库，而不使用迁移。  如果数据库不存在，则创建数据库。<br/>有一个参数`--skip-generate`:不生成Prisma Client |
| migrate  dev | 生成迁移（例如SQL）  生成Prisma Client   迁移+db push +  generate |
| studio       |                                                              |



参考[官方文档](https://www.prisma.io/docs/orm/reference/prisma-cli-reference#init)



## Scheme

Relations：参考[官方文档](https://www.prisma.io/docs/orm/prisma-schema/data-model/relations)



## 填充数据库

见[教程](https://www.prisma.io/blog/nestjs-prisma-rest-api-7D056s1BmOL0#seed-the-database)



