---
title: "Typescript"
description: 
date: 2023-12-27T15:15:35+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# 脚手架



安装Node.js

安装TS

```bash
npm install -g typescript
```

在一个空文件夹

```bash
npm init -y
```

创建Git

```bash
git init
echo "node_modules" > .gitignore 
```

创建ts配置文件

```bash
tsc --init
```

安装ts-node

```bash
npm install ts-node typescript --save-dev
```

运行ts文件

```bash
npx ts-node index.ts
```



在package.json添加

```json
"scripts": {
  "start": "ts-node index.ts"
}
```

使用npm运行ts文件

```bash
npm start
```



