---
title: "生产级LLM工具"
description: 
date: 2024-01-18T13:31:39+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: true
---



# 编程方式

由于这一波 LLMs 强大的理解、生成能力，关注细节的命令式编程似乎不再需要，而偏重流程或者说业务逻辑编排的 pipeline 能力的声明式编程，成了主流「编程」方式，所以我们看到包括 LCEL、DSPy、Semantic-kernel 在内的新秀都开始「向上」关注，把主要精力放在了业务逻辑编排能力上，或许这预示着类似 Airflow 的 DAG 方式，会变成新一代的编程范式？





# 总体

Prompt工具

LangSmith或Azure Prompt Flow



编程语言

Python或Javascript



开发框架/编排工具

Langchain或Semitic Kernel



API框架/服务框构架

FastAPI或Next.js



云托管厂商

Render（AWS Lamada）或Vercel



# 构建LLM应用

| 分类             | Name                                                         | Star数量 | 备注                                                         | 学习                                                       |
| ---------------- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 框架             | LangChain                                                    | 77       | 【必学】 | * |
|                  | [LlamaIndex](https://github.com/run-llama/llama_index)       | 29       | 【必学】专注于RAG，Python，JS SDK比较差                          | *                         |
|                  | [Semantic-kernel](https://github.com/microsoft/semantic-kernel?tab=readme-ov-file) | 17       | 【不确定？】C#，Python，Java                                       | ?                                      |
|                  | [haystack](https://github.com/deepset-ai/haystack)           | 13       | 有了LangChain可能就不需要这个了 |  |
|  | [DSPy](https://github.com/stanfordnlp/dspy?tab=readme-ov-file) | 7        | 太学术了？The framework for programming—not prompting—foundation models |  |
| Agent            | [SuperAgent](https://github.com/homanp/superagent)           | 4        | 这个感觉很专业，有时间再学习                                        | ?                                       |
|                  | [Rift](https://github.com/morph-labs/rift?tab=Apache-2.0-1-ov-file#readme) | 3        | 专门针对程序员                                               |                                                |
|                  | [AutoGen](https://github.com/microsoft/autogen)              | 22       |                                                              |                                                              |
|                  | [Camel](https://github.com/camel-ai/camel)                   | 4        |                                                              |                                                              |
| GPTs             | [OpenGPTs](https://github.com/langchain-ai/opengpts)         | 6        | LangChain出品                                                |                                                 |
| 低代码           | LangFlow                                                     |          | LangChain可视化                                              |                                               |
|                  | [LiteLLM](https://github.com/langfuse/langfuse/blob/main/https:/langfuse.comdocs/litellm) |          |                                                              |                                                              |
|                  | [Flowise](https://flowiseai.com/)                            | 21       |                                                              |                                                              |
|                  | [Langflow](https://www.langflow.org/)                        | 14       |                                                              |                                                              |
|                  | [PromptChainer](https://promptchainer.io/)                   | 商业     | 感觉做的很专业                                               |                                                |
| RAG              | [vectara](https://vectara.com/)                              | 闭源     | 非常专业的RAG商业软件；也有[开源项目](https://github.com/vectara/vectara-answer)；用户可以将PDF、Word、PPT、RTF等文件数据上传至Vectara平台中，构建数据搜索引擎。 |  |



归档和观察
| 分类             | Name                                                         | Star数量 | 备注                                                         |
| ---------------- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ |
| 观察 | Obsidian Copilot                                             |          | 一个有趣的方法，用于如何使用语义搜索和 OpenSearch 的 BM25 实现 |
|      | [Tanuki](https://github.com/Tanuki/tanuki.py)                |          | 一个使用装饰器进行数据验证的 LLM 框架                         |
|      | [griptape](https://github.com/griptape-ai/griptape)          | 1.5      | 一个具有稍微更好的内部编码标准的 langchain 替代品            |
|  | [guidance](https://github.com/guidance-ai/guidance)          | 16       | 比传统的提示或链接更有效地控制现代语言模型                   |
|  | [intructor](https://github.com/jxnl/instructor)              | 3.6      |                                                              |
|  | [Llmflows](https://github.com/stoyan-stoyanov/llmflows) | 0.5 | langchain 的另一个替代品，但在定义工作流程方面有一个有趣的方法 |
|  | [txtai](https://github.com/neuml/txtai) | 6.5 | 有用于聊天、医学/科学论文工作流程、开发者语义搜索以及标题和故事文本语义搜索的衍生产品 |
|  | [LiteLLM](https://github.com/BerriAI/litellm) | 5.7 | 一个简单、轻量级的 LLM 封装器 |
| 归档 | [Chidori](https://github.com/ThousandBirdsInc/chidori)       | 1.2      |                                                              |
|                  | [AutoChains](https://github.com/Forethought-Technologies/AutoChain) |          | 已经3个月没更新                                              |
|                  | [MiniChain](https://github.com/srush/MiniChain)              | 1.2      | 已经3个月没更新                                              |
|                  | [SimpleAIChat]()                                             | 3.3      | 已经7个月没更新                                              |



# 监控和分析



生产级别的LLM服务需要：

1. 调试 Prompt

2. Prompt 版本管理

3. 测试/验证系统的相关指标

4. 数据集管理

5. 各种指标监控与统计：访问量、响应时长、Token费用等



| 分类 | Name                                             | Star数量 | 备注 |
| ---- | ------------------------------------------------ | -------- | ---- |
| SaaS | LangSmith                                        | N/A      |      |
| 开源 | [LangFuse](https://github.com/langfuse/langfuse) | 2        |      |
| 开源 | Prompt Flow                                      | 7        |      |
|      | [Helicone](https://www.helicone.ai/)             | 1.2      |      |



生产级 LLM App 维护平台

1. **LangSmith**: LangChain 的官方平台，SaaS 服务，**非开源**
2. **[LangFuse](https://github.com/langfuse/langfuse)**: **开源** + SaaS，LangSmith 平替，可集成 LangChain ，对接 OpenAI API(2.1K)
3. **[Prompt Flow](https://github.com/microsoft/promptflow?tab=readme-ov-file)**：微软**开源** + Azure AI云服务，可集成 Semantic Kernel（7.4K）



Prompt Flow

学习

https://microsoft.github.io/promptflow/how-to-guides/quick-start.html





# UI组件

[LangUI](https://github.com/ahmadbilaldev/langui)(1.6K)

[agentlabs](https://github.com/agentlabs-inc/agentlabs)(0.5K)



# 其它

| 分类            | Name   | Star数量 | 备注 |
| --------------- | ------ | -------- | ---- |
| LCEL            |        |          |      |
| Python 模板语言 | Jinja2 |          |      |
|                 |        |          |      |







# 参考



| 分类 | Name                                                         | Star数量 | 备注                                                         |      |
| ---- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ | ---- |
| 框架 | Obsidian Copilot                                             |          | 一个有趣的方法，用于如何使用语义搜索和 OpenSearch 的 BM25 实现 |      |
|      | [Tanuki](https://github.com/Tanuki/tanuki.py)                |          | 个使用装饰器进行数据验证的 LLM 框架                          |      |
|      | [LiteLLM](https://github.com/BerriAI/litellm)                | 5.7      | 一个简单、轻量级的 LLM 封装器                                |      |
|      | [AutoChain](https://github.com/Forethought-Technologies/AutoChain) | 1.7      | langchain 的另一个替代品                                     |      |
|      | [griptape](https://github.com/griptape-ai/griptape)          | 1.5      | 一个具有稍微更好的内部编码标准的 langchain 替代品            |      |
|      | [txtai](https://github.com/neuml/txtai)                      | 6.5      | has spinoffs for chat, workflows for medical/scientific papers, semantic search for developers and semantic search for headlines and story text<br />有用于聊天、医学/科学论文工作流程、开发者语义搜索以及标题和故事文本语义搜索的衍生产品 |      |
|      | [Llmflows](https://github.com/stoyan-stoyanov/llmflows)      | 0.5      | langchain 的另一个替代品，但在定义工作流程方面有一个有趣的方法 |      |
|      | [guidance](https://github.com/guidance-ai/guidance)          | 16       | 比传统的提示或链接更有效地控制现代语言模型                   | *    |
| 库   | [intructor](https://github.com/jxnl/instructor)              | 3.6      |                                                              | *    |



**E2B：Frameworks and tools for AI products**



| Category                             | Tools/Platforms                                              |
| ------------------------------------ | ------------------------------------------------------------ |
| Monitoring, Observability, Analytics | Langsmith, Helicone, langfuse, [context](https://context.ai/) |
| Frontend                             | [AgentLabs](https://github.com/agentlabs-inc/agentlabs)      |
| Runtime for LLMs                     | [E2B](https://e2b.dev/ai-agents/closed-source)               |
| Building frameworks & platforms      | Langchain, Haystack by deepset, Superagent, Poe, [GenWorlds](https://github.com/yeagerai/genworlds), LiteLLM, [Rift by Morph](https://github.com/morph-labs/rift?tab=Apache-2.0-1-ov-file#readme), AutoGen, Vercel AI SDK, OpenGPTs, Huggging Face Agents, [Fixie](https://www.fixie.ai/) |
| Data integration, memory management  | vectara, LlamaIndex, ABACUS.AI, [cadea](https://www.cadea.ai/), [SID Memory](https://www.sid.ai/memory) |
| API and routers for LLMs             | [Genoss GPT](https://genoss.ai/), [Martian](https://withmartian.com/), [OpenRouter](https://openrouter.ai/) |
| Libraries for building AI products   | CAMEL, [ChatterBot](https://github.com/gunthercox/ChatterBot)（5年没更新了） |
| Orchestration                        | AGIXT                                                        |
| Building & deploying LLMs            | [BANANA](https://www.banana.dev/#pricing)                    |



![frameworks and tools for AI products](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/frameworks%20and%20tools%20for%20AI%20products.png)

