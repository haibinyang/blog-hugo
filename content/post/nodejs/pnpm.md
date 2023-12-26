---
title: "pnpm和pnmp workspace"
description: 
date: 2023-12-26T09:15:39+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# 包管理器

常见有：

- npm
- pnpm
- yarn

# 扁平结构和嵌套结构



| 结构类型 | 描述                                                         | 优点                                                     | 缺点                                                         |
| -------- | ------------------------------------------------------------ | -------------------------------------------------------- | ------------------------------------------------------------ |
| 扁平结构 | 安装一个包时，该包的所有依赖包都会被安装到与该包同级的目录下。<br/>例如，安装express包后，`node_modules`目录下会出现express以外的多个包。 | 1. 便于管理和访问依赖包。<br>2. 避免重复安装相同的包。   | 1. 依赖结构不确定（不同包依赖同一包的不同版本时，最终安装的版本具有不确定性），可通过lock文件确定安装版本。<br>2. 扁平化算法复杂，耗时较长。<br>3. 可能非法访问未声明的包。 |
| 嵌套结构 | 一个包的依赖包会安装在这个包的`node_modules`目录下，<br/>依赖的依赖则安装在依赖包的`node_modules`目录下，依此类推。 | 1. 保持了依赖包的版本独立性。<br>2. 明确了包的依赖关系。 | 1. 包文件的目录可能非常长，导致路径问题。<br>2. 同一个包可能在不同地方被重复安装。<br>3. 相同包的实例不能共享，增加了空间占用。 |



**嵌套结构**

```
`node_modules`
├─ foo
  ├─ `node_modules`
     ├─ bar
       ├─ index.js
       └─ package.json
  ├─ index.js
  └─ package.json
```



# pnpm的原理

## 全局store

启用pnpm后，会在`node_modules`出现一个配置文件.modules.yaml

- `本机的pnpm store`的地址
- 当前项目的`.pnpm文件夹`的名称

![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/Snipaste_2023-12-26_09-45-09.png)



`.pnpm文件夹`中的依赖包硬连接到`本机的pnpm store`

> 软链接可以理解成快捷方式。 它和windows下的快捷方式的作用是一样的。 
>
> 硬链接等于`cp -p` 加 `同步更新`。即文件大小和创建时间与源文件相同，源文件修改，硬链接的文件会同步更新。可以防止别人误删你的源文件。
>
> ![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231226095139.png)



## 核心

当我们安装`bar`包时

- 根目录（`node_modules`）下只包含安装的包`bar`
  - 不包含bar依赖的第三方库
- 在`.pnpm文件夹`中创建bar文件夹，并创建子文件夹`node_modules`
  - 在子文件夹的`node_modules`中安装bar依赖的所有库
    - bar直接硬连接到`本机的pnpm store`
    - 依赖的库（例如foo）提升到`.pnpm文件夹`：foo硬链接到``本机的pnpm store``
    - 子目录`node_modules`中的foo软链接到`本机的pnpm store`中的foo
    - 根目录
- 安装另外一个pao包时，如果也依赖foo，pao/node_modules/foo也是软链接到`本机的pnpm store`中的foo



## 优点

软链接解决了磁盘空间占用的问题

硬链接解决了包的同步更新和统一管理问题。

将安装包和依赖包放在同一级目录下，即.pnpm/依赖包/node_modules下。防止了 **`依赖包间的非法访问`**，根据**Node模块路径解析规则**可知，不在安装包同级的依赖包无法被访问，即只能访问安装包依赖的包。



# 参考

https://mp.weixin.qq.com/s/mOkOQqMwRxz5P0TBchTcfw