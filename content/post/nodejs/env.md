---
title: "环境变量"
description: 
date: 2024-03-27T15:33:11+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



安装库

```
npm install dotenv
```



创建.evn文件

```
DB_HOST=localhost
DB_USER=root
DB_PASS=s1mpl3
```



加载

```
import * as dotenv from 'dotenv';
dotenv.config();
```



选择指定的文件

```

dotenv.config({ path: './.env.dev' })dotenv.config({ path: './.env.dev' });
```





使用

```
console.log(process.env.DB_HOST); // 输出: localhost
```

