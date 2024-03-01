---
title: "LangSmith"
description: 
date: 2024-02-29T11:33:19+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: true
---



# Typescript版本

[npm下载地址](https://www.npmjs.com/package/langchain)



## 安装

```bash
npm install langchain @langchain/core @langchain/community @langchain/openai langsmith
```

> LangChain所有第三方的库：[链接](https://www.npmjs.com/search?ranking=popularity&q=%40langchain)





## 创建数据集

手动添加

批量添加



## 跟踪

```js
import { ChatOpenAI } from "@langchain/openai";
import { ChatPromptTemplate } from "@langchain/core/prompts";
import { StringOutputParser } from "@langchain/core/output_parsers";

async function main() {
    const prompt = ChatPromptTemplate.fromMessages([
        ["system", "You are a helpful assistant. Please respond to the user's request only based on the given context."],
        ["user", "Question: {question}\nContext: {context}"],
    ]);
    const model = new ChatOpenAI({ modelName: "gpt-3.5-turbo" });
    const outputParser = new StringOutputParser();

    const chain = prompt.pipe(model).pipe(outputParser);

    const question = "Can you summarize this morning's meetings?"
    const context = "During this morning's meeting, we solved all world conflict."
    await chain.invoke({ question: question, context: context });
}

main();
```



## 评估

```ts
import { Client } from "langsmith";
import { StringOutputParser } from "@langchain/core/output_parsers";
import { ChatPromptTemplate } from "@langchain/core/prompts";
import { ChatOpenAI } from "@langchain/openai";
import { RunEvalConfig, runOnDataset } from "langchain/smith";
import { Run, Example } from "langsmith";
import { EvaluationResult } from "langsmith/evaluation";

async function main() {
  // 待测试的函数
  const llm = new ChatOpenAI({ modelName: "gpt-3.5-turbo", temperature: 0 });
  const prompt = ChatPromptTemplate.fromMessages([
    ["human", "Spit some bars about {question}."],
  ]);
  const chain = prompt.pipe(llm).pipe(new StringOutputParser());

  // 定义和执行评估器
  const mustMention = async ({ run, example, }: { run: Run; example?: Example; })
    : Promise<EvaluationResult> => {
    // const mustMention: string[] = example?.outputs?.must_contain ?? [];
    // const score = mustMention.every((phrase) =>
    //   run?.outputs?.output.includes(phrase)
    // );
    return {
      key: "must_mention",
      score: 99,
    };
  };

  const evalConfig: RunEvalConfig = {
    customEvaluators: [mustMention],
  };

  let datasetName = "Rap Battle Dataset";
  await runOnDataset(chain, datasetName, {
    evaluationConfig: evalConfig
  });

}

main()
```





# Python版本

N/A
