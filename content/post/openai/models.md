---
title: "如何选择OpenAI的模型"
description: 
date: 2024-02-01T14:53:31+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---

# 动机

OpenAI的模型比较多，更新也比较频繁，用户不知道如何选择。

本文系统地整理了模型，可以快速地选择模型。



# 结论

如果要直接看结论，见下表。

| **分类**  | **训练数据截止时间** | **价格** | **响应速度** | **上下文长度** | **其它**               | **选择的模型名称**       |
| --------- | -------------------- | -------- | ------------ | -------------- | ---------------------- | ------------------------ |
| GPT-4     | 2023年12月           | 贵       | 慢           | 128K           |                        | **gpt-4-turbo-preview**  |
|           |                      |          |              |                | 视觉能力               | gpt-4-vision-preview     |
| GPT-3.5   | 2021年9月            | 便宜     | 快           | 16K            |                        | **gpt-3.5-turbo**        |
| Embedding |                      |          |              |                |                        | `text-embedding-3-small` |
|           |                      |          |              |                | 希望非英文的场景更精确 | `text-embedding-3-large` |



# 截止时间

本文章的数据，截止到2024年2月21日。



# 模型分类

| 模型名称   | 说明     |
| :--------- | -------- |
| GPT-4      | 文生文   |
| GPT-3.5    |          |
| DALL·E     | 文生图   |
| Sora       | 文生视频 |
| TTS        | 文成语音 |
| Whisper    | 识别语音 |
| Embeddings |          |
| Moderation | 审核内容 |



# GPT 3.5和4.0

| Model                     | Input(美元/1K tokens) | Output(美元/1K tokens) | 输入+输出 | 输出 | 训练数据截止时间 | 响应速度 | 其它       | 选择的模型名称          |
| ------------------------- | --------------------- | ---------------------- | --------- | ---- | ---------------- | -------- | ---------- | ----------------------- |
| gpt-3.5-turbo-instruct    | 0.0015                | 0.002                  | 4K        | N/A  | 2021年9月        | 快       |            | gpt-3.5-turbo-instruct  |
| gpt-3.5-turbo-0125        | 0.0005                | 0.0015                 | **16K**   | 4K   | 2021年9月        | 快       |            | **gpt-3.5-turbo**       |
|                           |                       |                        |           |      |                  |          |            |                         |
| gpt-4-0125-preview        | 0.01                  | 0.03                   | 128K      | 4K   | **2023年12月**   | 慢       |            | **gpt-4-turbo-preview** |
| gpt-4-1106-vision-preview | 0.01                  | 0.03                   | 128K      | 4K   | 2023年4月        |          | 有视觉能力 | gpt-4-vision-preview    |

> **gpt-4-turbo-preview**：训练数据已经到了2023年12月。
>
> **gpt-3.5-turbo-instruct**：已经支持到16K



# Embedding

| 模型名称               | 描述                               | 向量大小 | 价格          |
| :--------------------- | :--------------------------------- | :------- | ------------- |
| text-embedding-3-large | 适合非英文的任务。                 | 3,072    | 比ada稍微多点 |
| text-embedding-3-small | text-embedding-ada-002的升级版本。 | 1,536    | 是ada的1/5    |
| text-embedding-ada-002 |                                    | 1,536    |               |





# ~~GPT 3.5~~

~~截止到2024年2月19日，如果想使用GPT-3.5，使用`gpt-3.5-turbo`就可以。~~



| ~~模型名称~~               | ~~说明~~                          | ~~备注~~ |
| :------------------------- | :-------------------------------- | -------- |
| ~~gpt-3.5-turbo~~          | ~~指向 `gpt-3.5-turbo-0125`~~     |          |
| ~~gpt-3.5-turbo-16k~~      | ~~指向 `gpt-3.5-turbo-16k-0125`~~ |          |
|                            |                                   |          |
| ~~gpt-3.5-turbo-0125~~     |                                   |          |
| ~~gpt-3.5-turbo-1106~~     | ~~最新版本（截止到2024年1月）~~   |          |
|                            |                                   |          |
| ~~gpt-3.5-turbo-0613~~     |                                   |          |
| ~~gpt-3.5-turbo-16k-0613~~ |                                   |          |
| ~~gpt-3.5-turbo-0301~~     |                                   |          |
| ~~gpt-3.5-turbo-instruct~~ |                                   |          |

> ~~训练数据截止到：2021年9月~~
>
> ~~tokens限制：默认是4,096 tokens，如果名字中有16K，就是16,385 tokens。~~



~~**gpt-3.5-turbo-1106更新的内容**~~

~~具有改进的指令跟随、JSON模式、可重现输出、并行函数调用等功能。[了解更多](https://openai.com/blog/new-models-and-developer-products-announced-at-devday)。~~

> ~~In addition to GPT-4 Turbo, we are also releasing a new version of GPT-3.5 Turbo that supports a 16K context window by default. The new 3.5 Turbo supports improved instruction following, JSON mode, and parallel function calling. For instance, our internal evals show a 38% improvement on format following tasks such as generating JSON, XML and YAML. Developers can access this new model by calling `gpt-3.5-turbo-1106` in the API. Older models will continue to be accessible by passing `gpt-3.5-turbo-0613` in the API until June 13, 2024. [Learn more](https://platform.openai.com/docs/models/gpt-3-5).~~



# ~~GPT-4~~

~~截止到2024年2月1日，如果想使用GPT-4，使用`gpt-4-turbo-preview`就可以；如果需要视觉能力就使用`gpt-4-vision-preview`。~~



| ~~MODEL~~                | ~~DESCRIPTION~~                   |                                                              |
| :----------------------- | :-------------------------------- | ------------------------------------------------------------ |
| ~~gpt-4-turbo-preview~~  | ~~当前指向 `gpt-4-0125-preview`~~ | ~~最新的模型（截止到2024年1月）~~                            |
| ~~gpt-4~~                | ~~当前指向 `gpt-4-0613`~~         | ~~很老的模型，不是最新的模型。暂时不要用。~~                 |
| ~~gpt-4-32k~~            | ~~当前指向 `gpt-4-32k-0613`~~     | ~~很老的模型，不是最新的模型。暂时不要用。~~                 |
| ~~gpt-4-vision-preview~~ |                                   | ~~使用视觉能力的话，必须使用这个。~~                         |
|                          |                                   |                                                              |
| ~~gpt-4-0125-preview~~   | ~~最新的模型（截止到2024年1月）~~ | ~~不用使用这个，使用`gpt-4-turbo-preview`这个别名就可以了。~~ |
| ~~gpt-4-1106-preview~~   |                                   |                                                              |
|                          |                                   |                                                              |
| ~~gpt-4-0613~~           |                                   |                                                              |
| ~~gpt-4-32k-0613~~       |                                   |                                                              |

> ~~除了老的GPT-4，其它（turbo或preview）模块：~~
>
> - ~~训练数据截止时间：2023年3月~~
> - ~~tokens限制：总共是128K；但，输出只有4K。~~





# 模型更新说明

- [2024年1月](https://openai.com/blog/new-embedding-models-and-api-updates)
- [2023年11月](https://openai.com/blog/new-models-and-developer-products-announced-at-devday)

