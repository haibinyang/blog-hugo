---
title: "Monorepo入门"
description: 
date: 2023-12-26T08:51:54+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# MonoRepo

> - 英 [ˈmɒnəʊ]
> - 美 [ˈmɑnoʊ]



## 演进

| 阶段                                 | 简单描述                                                     |
| ------------------------------------ | ------------------------------------------------------------ |
| 阶段一：单仓库巨石应用 (Monolith)    | 一个 Git 仓库包含所有项目代码，随业务复杂度增加，代码量增大，导致效率下降。 |
| 阶段二：多仓库多模块应用 (MultiRepo) | 项目拆分为多个模块，分别在不同的仓库管理，提高了效率，简化了代码管理。 |
| 阶段三：单仓库多模块应用 (MonoRepo)  | 为管理方便，多个模块合并到一个仓库，解决了多仓库管理的问题，实现了快捷的代码共享和依赖管理。 |



![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231226090218.png)



## 优缺点



![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231226090552.png)

| 场景       | MultiRepo                                                    | MonoRepo                                                     |
| :--------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| 代码可见性 | ✅ 代码隔离，研发者只需关注自己负责的仓库 ❌ 包管理按照各自owner划分，当出现问题时，需要到依赖包中进行判断并解决。 | ✅ 一个仓库中多个相关项目，很容易看到整个代码库的变化趋势，更好的团队协作。 ❌ 增加了非owner改动代码的风险 |
| 依赖管理   | ❌ 多个仓库都有自己的 node_modules，存在依赖重复安装情况，占用磁盘内存大。 | ✅ 多项目代码都在一个仓库中，相同版本依赖提升到顶层只安装一次，节省磁盘内存， |
| 代码权限   | ✅ 各项目单独仓库，不会出现代码被误改的情况，单个项目出现问题不会影响其他项目。 | ❌ 多个项目代码都在一个仓库中，没有项目粒度的权限管控，一个项目出问题，可能影响所有项目。 |
| 开发迭代   | ✅ 仓库体积小，模块划分清晰，可维护性强。 ❌ 多仓库来回切换（编辑器及命令行），项目多的话效率很低。多仓库见存在依赖时，需要手动 `npm link`，操作繁琐。 ❌ 依赖管理不便，多个依赖可能在多个仓库中存在不同版本，重复安装，npm link 时不同项目的依赖会存在冲突。 | ✅ 多个项目都在一个仓库中，可看到相关项目全貌，编码非常方便。 ✅ 代码复用高，方便进行代码重构。 ❌ 多项目在一个仓库中，代码体积多大几个 G，`git clone`时间较长。 ✅ 依赖调试方便，依赖包迭代场景下，借助工具自动 npm link，直接使用最新版本依赖，简化了操作流程。 |
| 工程配置   | ❌ 各项目构建、打包、代码校验都各自维护，不一致时会导致代码差异或构建差异。 | ✅ 多项目在一个仓库，工程配置一致，代码质量标准及风格也很容易一致。 |
| 构建部署   | ❌ 多个项目间存在依赖，部署时需要手动到不同的仓库根据先后顺序去修改版本及进行部署，操作繁琐效率低。 | ✅ 构建性 Monorepo 工具可以配置依赖项目的构建优先级，可以实现一次命令完成所有的部署。 |



## 一个典型的MonoRepo目录

```json
├── apps
|   ├── web
|   |   ├── package.json
├── packages
|   ├── utils
|   |   ├── package.json
|   ├── ui
|   |   ├── package.json
├── pnpm-workspace.yaml
├── package.json
├── turbo.json
```



## 问题和解决方案



| 标题           | 问题                                                         | 解决方案                                                     |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 幽灵依赖       | npm/yarn 安装依赖时，存在依赖提升现象。某项目使用的依赖没有在其 package.json 中声明，但也可以直接使用，称为“幽灵依赖”。随着项目迭代，依赖不再被其他项目使用，不再安装，导致使用幽灵依赖的项目报错。 | 使用 pnpm 可彻底解决基于 npm/yarn 的 Monorepo 方案中的“幽灵依赖”问题。 |
| 依赖安装耗时长 | MonoRepo 中每个项目都有自己的 package.json 依赖列表，依赖总数增长导致每次 install 时耗时长。 | 将相同版本依赖提升至 Monorepo 根目录下，减少冗余依赖安装；使用 pnpm 按需安装及依赖缓存。 |
| 构建打包耗时长 | 多个项目构建任务存在依赖时，通常是串行构建或全量构建，导致构建时间较长。 | 实施增量构建，而非全量构建；或将串行构建优化为并行构建。     |



## 技术选型



| 工具     | TurboRepo | Rush | Nx   | Lerna | pnpm Workspace |
| :------- | :-------- | :--- | :--- | :---- | :------------- |
| 依赖管理 | ❌         | ✅    | ❌    | ❌     | ✅              |
| 版本管理 | ❌         | ✅    | ❌    | ✅     | ❌              |
| 增量构建 | ✅         | ✅    | ✅    | ❌     | ❌              |
| 插件扩展 | ✅         | ✅    | ✅    | ❌     | ❌              |
| 云端缓存 | ✅         | ✅    | ✅    | ❌     | ❌              |
| Stars    | 20.4K     | 4.9K | 17K  | 34.3K | 22.7K          |

TurboRepo支持pnpm，我选择：`pnpm workspace`+`TurboRepo`。



## 参考

https://mp.weixin.qq.com/s/kGfNh_yyzGy_F4OkxNlqeA