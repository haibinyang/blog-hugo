---
title: "LangChain"
description: 
date: 2024-02-28T11:33:19+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: true
---



# 阅读官方文档计划

| 分类1     | 分类2          | 进展 |
| --------- | -------------- | ---- |
| LCEL      | Interface      |      |
|  | Streaming | |
|  | How to | Route between multiple runnables✅<br/>Cancelling requests✅<br/>Use RunnableMaps✅<br/>Add message history (memory) |
|           | Cookbook       | ✅Prompt + LLM<br/>✅Multiple chains<br/>✅Retrieval augmented generation (RAG)<br/>✅Querying a SQL DB<br/>Adding memory<br/>✅Using tools<br/>Agents |
| Model I/O | Quickstart | |
| | Concepts | ✅ |
|  | Prompts        | Quick Start<br/>Example selectors<br/>Few Shot Prompt Templates<br/>Partial prompt templates<br/>Composition |
|           | LLMs           | Quick Start<br/>Streaming<br/>Caching<br/>Custom chat models<br/>Tracking token usage<br/>Cancelling requests<br/>Dealing with API Errors<br/>Dealing with rate limits<br/>OpenAI Function calling<br/>Subscribing to events<br/>Adding a timeout |
|           | Chat Models    |      |
|           | Output Parsers | ✅ |
| Retrieval | 首页/概念 |      |
|  | Document loaders | |
|  | Text Splitters | |
|  | Retrievers | |
|  | Text embedding models | |
|  | Vector stores | |
|  | Indexing | |
|  | Experimental | |
| Chains    |                | ✅ |
| Agents    |                |      |
| More      |                |      |
| Guides    |                |      |
| User cases | SQL | |
|  | Chatbots |  |
|  | Q&A with RAG |  |
|  | Tool use |  |
|  | Interacting with APIs |  |
|  | Tabular Question Answering |  |
|  | Summarization |  |
|  | Agent Simulations |  |
|  | Autonomous Agents |  |
|  | Code Understanding |  |
|  | Extraction |  |




# LangChain生态

优点：支持Javascript，这点比LllamaIndex强很多（llamda虽然支持ts但是文档和API明显比Python版本差很多）

生态：



<img src="https://python.langchain.com/assets/images/langchain_stack-f21828069f74484521f38199910007c1.svg" alt="Diagram outlining the hierarchical organization of the LangChain framework, displaying the interconnected parts across multiple layers." style="zoom:33%;" />

# 概念

## LLM和Chat Model

Models：包含两种类型LLMs和Chat Models。



```js
import { OpenAI, ChatOpenAI } from "@langchain/openai";

const llm = new OpenAI({
  modelName: "gpt-3.5-turbo-instruct",
});

const chatModel = new ChatOpenAI({
  modelName: "gpt-3.5-turbo",
});
```



> Anthropic 的模型最适合 XML，而 OpenAI 的模型最适合 JSON。



# Typescript版本

[npm下载地址](https://www.npmjs.com/package/langchain)



## 安装

```bash
npm install langchain @langchain/core @langchain/community @langchain/openai langsmith
```

> LangChain所有第三方的库：[链接](https://www.npmjs.com/search?ranking=popularity&q=%40langchain)





## Quick Start

```js
import { ChatOpenAI } from "@langchain/openai";

async function main() {
    const chatModel = new ChatOpenAI({});
    let str = await chatModel.invoke("what is LangSmith?");
    console.log(str);
}

main();
```



## Function Call

> [官方文档](https://js.langchain.com/docs/modules/model_io/chat/function_calling)

**两种方式**



调用时传入函数

```js
const result = await model.invoke([new HumanMessage("What a beautiful day!")], {
  functions: [extractionFunctionSchema],
  function_call: { name: "extractor" },
});
```



绑定函数到模型

可以不断复用同一个模型

```js
const model = new ChatOpenAI({ modelName: "gpt-4" }).bind({
  functions: [extractionFunctionSchema],
  function_call: { name: "extractor" },
});
```



定义函数有两种方式



```js
const extractionFunctionSchema = {
  name: "extractor",
  description: "Extracts fields from the input.",
  parameters: {
    type: "object",
    properties: {
      tone: {
        type: "string",
        enum: ["positive", "negative"],
        description: "The overall tone of the input",
      },
      word_count: {
        type: "number",
        description: "The number of words in the input",
      },
      chat_response: {
        type: "string",
        description: "A response to the human's input",
      },
    },
    required: ["tone", "word_count", "chat_response"],
  },
};
```



**使用Zod**

```js
import { ChatOpenAI } from "@langchain/openai";
import { z } from "zod";
import { zodToJsonSchema } from "zod-to-json-schema";
import { HumanMessage } from "@langchain/core/messages";

const extractionFunctionSchema = {
  name: "extractor",
  description: "Extracts fields from the input.",
  parameters: zodToJsonSchema(
    z.object({
      tone: z
        .enum(["positive", "negative"])
        .describe("The overall tone of the input"),
      entity: z.string().describe("The entity mentioned in the input"),
      word_count: z.number().describe("The number of words in the input"),
      chat_response: z.string().describe("A response to the human's input"),
      final_punctuation: z
        .optional(z.string())
        .describe("The final punctuation mark in the input, if any."),
    })
  ),
};
```



## Model I/O

### Parser

|          | 解析器                       | 说明       |
| -------- | ---------------------------- | ---------- |
| 常见     | String output parser         |            |
| 格式化   | Structured output parser     | 方便自定义 |
|          | OpenAI Tools                 | 常用       |
| 标准格式 | JSON Output Functions Parser | 常用       |
|          | HTTP Response Output Parser  |            |
|          | XML output parser            |            |
| 列表     | List parser                  | 常用       |
|          | Custom list parser           | 常用       |
| 其它     | Datetime parser              | 有用       |
|          | Auto-fixing parser           |            |



多种Chain

## 多个Chain

### 组合/串行

两种方式

-  `.pipe` 
-  `RunnableSequence.from([])` 



使用.pipe

```js
const prompt = ChatPromptTemplate.fromMessages([
  ["human", "Tell me a short joke about {topic}"],
]);
const model = new ChatOpenAI({});
const outputParser = new StringOutputParser();

const chain = prompt.pipe(model).pipe(outputParser);
const response = await chain.invoke({
  topic: "ice cream",
});
```



使用RunnableSequence.from

```js
const model = new ChatOpenAI({});
const promptTemplate = PromptTemplate.fromTemplate(
  "Tell me a joke about {topic}"
);

const chain = RunnableSequence.from([promptTemplate, model]);
const result = await chain.invoke({ topic: "bears" });
```



### 批量和并行

LCEL本身支持

```js
const chain = promptTemplate.pipe(model);
await chain.batch([{ topic: "bears" }, { topic: "cats" }])
```



使用RunnableMap

```js
const model = new ChatAnthropic({});
const jokeChain = PromptTemplate.fromTemplate(
  "Tell me a joke about {topic}"
).pipe(model);
const poemChain = PromptTemplate.fromTemplate(
  "write a 2-line poem about {topic}"
).pipe(model);

const mapChain = RunnableMap.from({
  joke: jokeChain,
  poem: poemChain,
});

const result = await mapChain.invoke({ topic: "bear" });
```







### 分支

两种方式

- RunnableBranch
- custom factory function



### 中止、重试、Fallback

N/A



### 典型例子：串行



```js
import { PromptTemplate } from "@langchain/core/prompts";
import { RunnableSequence } from "@langchain/core/runnables";
import { StringOutputParser } from "@langchain/core/output_parsers";
import { ChatOpenAI } from "@langchain/openai";

async function main() {
    const prompt1 = PromptTemplate.fromTemplate(
        `What is the city {person} is from? Only respond with the name of the city.`
    );
    const prompt2 = PromptTemplate.fromTemplate(
        `What country is the city {city} in? Respond in {language}.`
    );

    const model = new ChatOpenAI({});

    const chain1 = prompt1.pipe(model).pipe(new StringOutputParser());

    const combinedChain = RunnableSequence.from([
        {
            city: chain1,
            language: (input) => input.language,
        },
        prompt2,
        model,
        new StringOutputParser(),
    ]);

    const result = await combinedChain.invoke({
        person: "Obama",
        language: "German",
    });

    console.log(result);
}

main();
```



结果见[这里](https://smith.langchain.com/public/c8c812b2-c707-4da7-a1c5-8280c3d054e9/r)

![image-20240301094802136](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240301094802136.png)

![image-20240301095114942](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240301095114942.png)



# Python版本

安装[LangChain](https://python.langchain.com/docs/get_started/installation#official-release)全家桶

```
pip install langchain langchain-community langchain-core "langserve[all]" langchain-cli langsmith langchain-openai
```

> 最新版本号：[0.1.9](https://pypi.org/project/langchain/)（截止到2024年2月28日）



