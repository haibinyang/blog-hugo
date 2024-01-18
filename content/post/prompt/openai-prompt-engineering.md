---
title: "OpenAI的提示工程：官方指南"
description: 
date: 2024-01-18T11:33:19+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---





![image-20240118151238515](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240118151238515.png)

# 实例





# 清晰的指令





## 增加细节

在查询中包含详细信息以获得更相关的答案。



| **更 糟**                  | **更好**                                                     |
| -------------------------- | ------------------------------------------------------------ |
| 如何在Excel中添加数字？    | 如何在Excel中将一行美元金额相加？我想自动为整张行表执行此操作，所有总数都位于名为“Total”的列的右侧。 |
| 谁是总统？                 | 谁是 2021 年的墨西哥总统，选举多久举行一次？                 |
| 编写代码来计算斐波那契数列 | 编写一个 TypeScript 函数来有效地计算斐波那契数列。对代码进行大量注释，以解释每个部分的作用以及为什么这样编写。 |
| 总结会议记录               | 用一个段落总结会议记录。然后写一个演讲者和他们每个关键点的降价列表。最后，列出演讲者建议的后续步骤或行动项目（如果有的话）。 |



## 设定角色

要求模型采用角色。



**官方范例**

> 系统消息

```
当我请求帮助写东西时，你会回复一份文档，每个段落中至少包含一个笑话或俏皮的评论。
```

> 用户消息

```
给我的钢螺栓供应商写一封感谢信，感谢他们在短时间内按时交货。这使我们能够交付一个重要的订单。
```

> 响应

```
亲爱的[供应商名称]团队，

首先，我想说，如果有奥运会金牌颁给最快的螺栓供应，你们肯定会赢得比赛，而且还会打破世界记录！

我写这封信是想表达我们公司对你们团队的深深感激。你们在短时间内按时交付的钢螺栓不仅坚固，而且速度快得让光都自愧不如。我们知道，没有你们的超级英雄般的努力，我们是不可能在截止日期前完成我们的订单的。

你们的团队就像是那个在最后一刻出现的骑士，不仅救了公主，还救了整个王国。我们的客户对我们的及时交付感到非常满意，这在很大程度上要归功于你们的高效和专业。你们的螺栓就像是我们成功的秘密武器，虽然我们不能在产品说明书上这么写，但我们知道真相！

我们感激你们的响应速度，就像是有超能力一样，你们总是能够在我们最需要的时候出现。我们的合作关系就像是螺栓和螺母一样紧密，没有你们，我们就散架了！

再次感谢你们的出色工作和无与伦比的服务。我们期待着继续这种令人愉快的合作关系，并在未来的项目中再次体验你们的“闪电般”的交付速度。

最后，如果你们的螺栓能够参加马拉松，我敢打赌，它们会是第一个冲过终点线的，而且还不会出一滴汗！

真诚地，

[你的名字]
[你的职位]
[你的公司名称]
```



## 添加分隔符

使用分隔符清楚地指示输入的不同部分



官方实例1：

> USER

```
Summarize the text delimited by triple quotes with a haiku.

"""insert text here"""
```



官方实例2：

> SYSTEM

```
You will be provided with a pair of articles (delimited with XML tags) about the same topic. First summarize the arguments of each article. Then indicate which of them makes a better argument and explain why.
```

> USER

```
<article> insert first article here </article>

<article> insert second article here </article>
```



官方实例3：

> SYSTEM

```
You will be provided with a thesis abstract and a suggested title for it. The thesis title should give the reader a good idea of the topic of the thesis but should also be eye-catching. If the title does not meet these criteria, suggest 5 alternatives.
```

> USER

```json
Abstract: insert abstract here

Title: insert title here
```



## 分解步骤

指定完成任务所需的步骤

> SYSTEM

```
使用以下分步说明来响应用户输入。
第 1 步 - 用户将为您提供三引号中的文本。用一句话总结这段文字，前缀是“摘要：”。
第 2 步 - 将第 1 步中的摘要翻译成西班牙语，前缀为“翻译：”。
```

> USER

```
"""提供适用于所有示例的一般说明通常比通过示例演示任务的所有排列更有效，但在某些情况下，提供示例可能更容易。例如，如果您打算让模型复制响应用户查询的特定样式，这很难显式描述。这被称为“小镜头”提示。"""
```

> 输出

```
摘要：通常提供通用说明比展示所有示例更有效，但在某些情况下，如模型需要复制特定样式的响应，提供示例可能更简单，这称为“小镜头”提示。

翻译：Generalmente, proporcionar instrucciones generales es más efectivo que mostrar todos los ejemplos, pero en algunos casos, como cuando el modelo necesita replicar respuestas de un estilo específico, ofrecer ejemplos puede ser más sencillo, lo que se conoce como "pistas de pequeño alcance".
```





## 举例说明/few-shot



> USER

```bash
以一致的风格回答。

Q：教我忍耐。
A：雕刻最深谷的河流从一汪不起眼的泉水流出;最伟大的交响曲起源于一个音符;最复杂的挂毯始于一根孤独的线。
Q：教我关于海洋的知识。
```

> 响应

```bash
A：宽阔的海洋藏匿着无尽的秘密，如同藏在古老卷轴里的智慧。它的蓝色深渊孕育着生命之源，正如星辰点缀夜空。海浪的起伏告诉我们自然的律动，如同风在林间吹拂的旋律。每一滴海水都承载着地球的故事，正如每个灵魂都编织着生命的篇章。
```





## 限制输出长度

指定所需的输出长度

```
Summarize the text delimited by triple quotes in 3 bullet points.
Summarize the text delimited by triple quotes in 2 paragraphs.
Summarize the text delimited by triple quotes in about 50 words. // 3 个要点
```



# 提供参考文本

## 指示模型使用参考文本进行回答

Instruct the model to answer using a reference text



> SYSTEM

```
使用提供的文章（用"""分隔）来回答问题。如果在文章中找不到答案，请写“我找不到答案”。
```

> USER

```bash
提供的文章：三星 Galaxy S 系列一直是安卓阵营的顶级旗舰，其中Ultra型号更是被称为“安卓之光”。在最新的S24 Ultra中，其最明显的变化就是机身采用了钛合金材料，类似于苹果的iPhone 15 Pro。然而，尽管S24 Ultra采用了更轻的钛合金材质，但它的重量并未减轻，与上一代的S23 Ultra几乎持平，并且比iPhone 15 Pro Max略重。
屏幕方面，S24 Ultra保持了与上一代相同的尺寸和分辨率，但其峰值亮度提高到了2600尼特。此外，S24 Ultra手机放弃了Note系列特有的曲面屏，转而采用了直屏设计。不过，与S24和S24+的完全直边屏幕不同，S24 Ultra的屏幕边缘仍然保留了轻微的弧度。
在影像方面，除了潜望式长焦摄像头外，S24 Ultra的其他四个摄像头在硬件上与上一代保持一致。S24 Ultra将上一代的1000万像素10倍光学变焦潜望式镜头进行了改进，换成了更高分辨率5000万像素潜望式长焦，支持五倍光学变焦。通过无损裁切，这个新镜头能够实现10倍的变焦效果。5000万像素潜望式长焦，支持五倍光学变焦
尽管三星S24 Ultra不再配备原生10倍光学变焦，但三星称S24 Ultra的10倍变焦图像质量实际上比上一代有所提升，并仍可以实现 100 倍变焦。S24 Ultra从10倍焦段改为5倍焦段，可能是因为5倍焦段在日常摄影中更为常用，且更便于构图。毕竟，一般用户很少需要用到10倍变焦，而且由于物理限制，超长焦潜望式镜头在光圈大小和CMOS传感器方面往往需要做出妥协。这导致了在低光环境下几乎无法使用，成像质量会大幅降低的现象。相比之下，5倍潜望式镜头可以采用更大的光圈和更大的CMOS传感器，在低光环境下的表现将会更佳。
AI 功能也是S24系列的一大亮点，三星表示，S24系列集成了几乎所有当前旗舰手机上的AI功能，并且这三款新机型在AI功能上完全一致。功能主要包括：AI驱动的图像和视频编辑工具、AI图像搜索识别、实时通话翻译、文本翻译、语音转文字翻译以及自动格式化笔记等等。

问题:  三星发布什么产品？
```

> 响应

```
三星发布了Galaxy S24 Ultra手机。
```



## 指示模型使用参考文本中的引文进行回答

Instruct the model to answer with citations from a reference text



```
SYSTEM
You will be provided with a document delimited by triple quotes and a question. Your task is to answer the question using only the provided document and to cite the passage(s) of the document used to answer the question. If the document does not contain the information needed to answer this question then simply write: "Insufficient information." If an answer to the question is provided, it must be annotated with a citation. Use the following format for to cite relevant passages ({"citation": …}).

USER
"""<insert document here>"""

Question: <insert question here>
```





# 拆解成子任务

Split complex tasks into simpler subtasks

将复杂的任务拆分为更简单的子任务



## 使用意向分类来识别与用户查询最相关的指令

Use intent classification to identify the most relevant instructions for a user query



## 对于需要很长对话的对话应用程序，请总结或筛选上一个对话

For dialogue applications that require very long conversations, summarize or filter previous dialogue



## （重点）分段总结长文档，递归构建完整摘要

Summarize long documents piecewise and construct a full summary recursively







# 给模型留出思考时间



## 在匆忙得出结论之前，指示模型制定自己的解决方案

Instruct the model to work out its own solution before rushing to a conclusion



## （未解决）使用内心独白或一系列查询来隐藏模型的推理过程

Use inner monologue or a sequence of queries to hide the model's reasoning process



## 询问模型在之前的传递中是否遗漏了任何内容

Ask the model if it missed anything on previous passes







# 使用外部工具



## RAG

使用基于嵌入的搜索实现高效的知识检索

Use embeddings-based search to implement efficient knowledge retrieval



## Code Interpreter

使用代码执行来执行更准确的计算或调用外部 API

Use code execution to perform more accurate calculations or call external APIs



## Function Call

Give the model access to specific functions







# 系统地测试更改



Evaluate model outputs with reference to gold-standard answers

参考黄金标准答案评估模型输出



# 参考

[参考](https://platform.openai.com/docs/guides/prompt-engineering/strategy-test-changes-systematically)