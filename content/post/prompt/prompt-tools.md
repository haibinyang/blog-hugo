---
title: "Prompt Tools"
description: 
date: 2024-01-18T13:31:39+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



生产级别的LLM服务需要：

1. 调试 Prompt

2. Prompt 版本管理

3. 测试/验证系统的相关指标

4. 数据集管理

5. 各种指标监控与统计：访问量、响应时长、Token费用等



生产级 LLM App 维护平台

1. **LangSmith**: LangChain 的官方平台，SaaS 服务，**非开源**
2. **[LangFuse](https://github.com/langfuse/langfuse)**: **开源** + SaaS，LangSmith 平替，可集成 LangChain ，对接 OpenAI API(1.8K)
3. **[Prompt Flow](https://github.com/microsoft/promptflow?tab=readme-ov-file)**：微软**开源** + Azure AI云服务，可集成 Semantic Kernel（7.1K）

还有，[LangUI](https://github.com/ahmadbilaldev/langui)(1.6K)



Prompt Flow

学习

https://microsoft.github.io/promptflow/how-to-guides/quick-start.html



