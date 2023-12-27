---
title: "Node.js相关"
description: 
date: 2023-12-27T12:19:41+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# dotenv-cli

[官网](https://github.com/entropitor/dotenv-cli)



## 指定.env文件

```bash
$ dotenv -e .env2 <command with arguments>
```



## 级联（Cascading）环境变量

作用

> 例如，如果你有 `.env` 和 `.env.local` 文件，你可以使用 `-c` 标志来同时加载这两个文件中的变量。
>
> 在这种情况下，`.env.local` 通常用于覆盖 `.env` 中的某些默认值，以适应本地机器或特定环境的需要。
>
> 好处是，你可以将通用的、非敏感的配置放在 `.env` 中，并将其纳入版本控制系统（如git），而将包含敏感信息的 `.env.local` 文件排除在版本控制之外。



使用`-c`参数

```
dotenv -e ../.env -c 
```



### 覆盖的优先级

1. `.env`
2. `.env.local`
3. `.env.development`
4. `.env.development.local`



.env.local：针对所有环境

.env.development：某一环境

.env.development.local：某一环境的local主机



[参考1](https://github.com/entropitor/dotenv-cli?tab=readme-ov-file#cascading-env-variables)

[参考2](https://github.com/entropitor/dotenv-cli/issues/37)



## 二级命令（underlying command ）

不使用dotenv-cli

```bash
mvn exec:java -Dexec.args="-g -f"
```

使用dotenv-cli

```bash
$ dotenv -- mvn exec:java -Dexec.args="-g -f"
```



