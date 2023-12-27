---
title: "Turborepo入门"
description: 
date: 2023-12-26T09:14:31+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# 第一个TurboRepo项目

## 初始化

新建目录`pnpm-workspace-demo`，执行`pnpm init`初始化项目，生成 **package.json**。

#### 指定Node和pnpm

为了减少因`node`或`pnpm`的版本的差异而产生开发环境错误，我们在package.json中增加`engines`字段来限制版本。

```json
{
    "engines": {
        "node": ">=16",
        "pnpm": ">=7"
    }
}
```



#### 安全性设置

为了防止我们的根目录被当作包发布，我们需要在package.json加入如下设置：

```json
{
    "private": true
}
```





## 创建workspace



1. 在根目录创建apps和packages两个文件夹。
2. 在apps中创建web文件夹，执行`pnpm init`初始化项目。
   - pacakge.json中的name设置为web
3. 在packages中创建utils和ui文件夹，执行`pnpm init`初始化项目。
   - pacakge.json中的name设置为utils和ui

```json
├── apps
|   ├── web
|   |   ├── package.json
├── packages
|   ├── utils
|   |   ├── package.json
|   ├── ui
|   |   ├── package.json
├── package.json
```



### 配置workspace

 根目录下新建`pnpm-workspace.yaml`文件，内容如下：

```yaml
packages:
  - 'apps/**'
  - 'packages/**'
```

这样pnpm才能识别出哪些是workspace。



## 安装依赖包

### 安装全局依赖包

格式

```bash
pnpm add --filter <workspace-name> <package-name>
```



```bash
pnpm add typescript -D -w
```

> `-w` 表示在workspace的根目录下安装而不是当前的目录
>
> `-D`表示安装在开发依赖

删除依赖包：`pnpm rm pkgname`



### 安装某个workspace的依赖

```bash
pnpm add --filter <workspace-name> <package-name>
```

> `--filter`或`-F`指定命令作用范围



## 使用TurboRepo来执行任务

### 安装`turbo`

```bash
pnpm install turbo -D -w
```



### 创建`turbo.json`文件



```json
{  "$schema": "https://turbo.build/schema.json"}
```



### 编辑 `.gitignore`

```yaml
# turbo
.turbo
```

### 使用turbo执行任务

```bash
turbo test
```



### 配置workspace的依赖关系

packages/utils/package.json

修改test任务：等待3秒并输出文本`utils:test`

```json
{
  "name": "utils",
  "scripts": {
    "test": "sleep 3 && echo \"utils: test\""
  }
  ...
}

```



packages/ui/package.json

修改test任务并依赖`utils`

```json
{
  "name": "ui",
  "scripts": {
    "test": "sleep 3 && echo \"utils: ui\""
  },
  "dependencies": {
    "utils": "workspace:^"
  }
  ...
}
```



apps/web/package.json

修改test任务并依赖`ui `

```json
{
  "name": "web",
  "scripts": {
    "test": "sleep 3 && echo \"web: test\""
  },
  "dependencies": {
    "ui": "workspace:^"
  }
  ...
}
```

### 编辑pipeline

- web的test任务
  - 要等ui的test任务
    - ui的test任务要等utils的test任务

```json
{
    "$schema": "https://turbo.build/schema.json",
    "pipeline": {
        "test": {
            "cache": false,
            "dependsOn": ["^test"]
        }
    }
}
```

执行任务

```bash
turbo test
```



### 环境变量

在根路径创建.env文件。

在根路径安装`dotenv-cli`。

```bash
pnpm add dotenv-cli -D -w
```

调整根目录的package.json的script，注入环境变量

```json
{
  "scripts": {
    "dev": "dotenv -- turbo dev"
  }
}
```

> `--` 符号通常用于指示命令行参数的结束，并开始传递给命令的子参数或子命令。在这个场景中，`--` 告诉 `dotenv` 命令，其后面的所有内容应该作为单独的命令执行，而不是作为 `dotenv` 的参数。



在turbo.json文件中增加.env文件，让turbo的hash算法感知到环境变量的变化。

```json
{
  "globalDotEnv": [".env"],
  "pipeline": {
    "dev": {
      "dependsOn": ["^build"]
    }
  }
}
```

> `globalEnv`：隐式全局哈希依赖项的环境变量列表。这些环境变量的内容将包含在全局哈希算法中，并影响所有任务的哈希值。
>
> `globalDependencies`：全局哈希依赖项的文件通配符列表。这些文件的内容将包含在全局哈希算法中，并影响所有任务的哈希值。这对于基于 `.env` 文件（不在 Git 中）或任何影响工作区任务的根级文件（但未在传统依赖关系图中表示（例如根 `tsconfig.json` 、、 `jest.config.js` `.eslintrc` 等））破坏缓存很有用。



### 限制并行任务的数量

默认10。

```bash
turbo run build --concurrency=50%
turbo run test --concurrency=1
```

[参考](https://turbo.build/repo/docs/reference/command-line-reference/run#--concurrency)



## 源码

本教程的源码见：

https://github.com/yhb-tutorial/pnpm-turborepo



# 参考

https://mp.weixin.qq.com/s/mOkOQqMwRxz5P0TBchTcfw

https://turbo.build/repo/docs/getting-started/existing-monorepo

https://turbo.build/repo/docs/core-concepts/monorepos/running-tasks