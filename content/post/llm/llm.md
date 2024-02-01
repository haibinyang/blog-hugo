---
title: "大模型"
description: 
date: 2024-01-18T11:33:19+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# LLM的用途

有常识

识别意图

分类

分步：状态机

补充函数的参数（厉害）



# 常用Prompt句式

## 总结

Summarize

delimited

```bash
Summarize the text delimited by triple quotes.

"""insert text here"""
```



as  follows

If applicable

```
You will be provided with meeting notes, and your task is to summarize the meeting as follows:
    
    -Overall summary of discussion
    -Action items (what needs to be done and who is doing it)
    -If applicable, a list of topics that need to be discussed more fully in the next meeting.
```



## 背景

You will be provided

```bash
You will be provided with xxx (delimited with XML tags) about xxx topic. 
First xxx. 
Then xxx and xxx.
```



## 分步



```bash
Use the following step-by-step instructions to respond to user inputs.

Step 1 - The user will provide you with text in triple quotes. Summarize this text in one sentence with a prefix that says "Summary: ".

Step 2 - Translate the summary from Step 1 into Spanish, with a prefix that says "Translation: ".
```



## 一致性



```bash
Answer in a consistent style.

Q: Teach me about patience.
A: The river that carves the deepest valley flows from a modest spring; the grandest symphony originates from a single note; the most intricate tapestry begins with a solitary thread.
Q: Teach me about the ocean.
```



## 限制长度

分 3 个要点

```
Summarize the text delimited by triple quotes in 3 bullet points.
Summarize the text delimited by triple quotes in 2 paragraphs.
Summarize the text delimited by triple quotes in about 50 words. // 3 个要点
```

## 限制范围

> 非常好

```
Draft a company memo to be distributed to all employees. The memo should cover the following specific points without deviating from the topics mentioned and not writing any fact which is not present here:
xxxx
```



## 分类

```
You will be provided with a tweet, and your task is to classify its sentiment as positive, neutral, or negative.
```



## 分类意图



```
You will be provided with customer service queries. Classify each query into a primary category and a secondary category. Provide your output in json format with the keys: primary and secondary.

Primary categories: Billing, Technical Support, Account Management, or General Inquiry.

Billing secondary categories:
- Unsubscribe or upgrade
- Add a payment method
- Explanation for charge
- Dispute a charge

Technical Support secondary categories:
- Troubleshooting
- Device compatibility
- Software updates

Account Management secondary categories:
- Password reset
- Update personal information
- Close account
- Account security

General Inquiry secondary categories:
- Product information
- Pricing
- Feedback
- Speak to a human
```



## 角色和任务

```
your task is to xxx it in a concise way.
```



## 简洁

```
explain it in a concise way.
```



## 目标用户

给一个二年级的学生

```
Summarize content you are provided with for a second-grade student.
```



## 格式

用圆点列表格式输出

```
Provide your answer in bullet point form. 
```

输出

- Easy to use
- Provides good value for the price
- High quality and durability
- Difficult to transport
- Difficult to store



有序的列表

a numbered list

```
create a numbered list of turn-by-turn directions from it.
```



# 基础和概念

## Prompt

> 中文翻译：提示词

给机器的指令，类似编程语言。



## LLM

> 中文翻译：大模型

本质：一套算法，类似一个函数。





### 常见的模型

- OpenAI: GPT-3.5/GPT-4/GPT-4V/DALL.EWhisper
- Meta: LLama2（开源）



## LLM的幻觉

由于模型在训练过程中面对的知识量是非常庞大的，它并不能完美地记住所有它见过的信息，一个很明显的问题就是，**模型并不清楚自己的知识边界**。

这就意味着，**模型可能会在回答一些晦涩难懂的话题时，编造听起来可信但实际并不正确的答案**，这种编造的答案我们称之为“幻觉”。

例如下面这个例子，当我们要求：

> ❝
>
> 告诉我关于Boy公司的AeroGlide Ultra Slim智能牙刷

其中，公司名是存在的，产品名称却是我们虚构的，在这种情况下，模型依旧会给出一个相当逼真的虚构产品描述。

减少这种幻觉的产生由2种可参考的策略：

- 策略1：要求模型基于提供的文本找到相关引用并回答问题
- 策略2：将答案追溯到源文件



## Temperature

> 翻译：温度

这是模型常见的参数，可选值是：0~1。

- 当Temperature为0时：代表回答更准确，更固定，适用于期望每次都得到相同的输出结果

- 当Temperature为0.7时：代表回答更随机，更有创造性，适用于期望每次都得到不同的输出结果

  

<img src="https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20240118101738.png" style="zoom: 50%;" />

> 例如，当回答“我的最爱食物是……”这个问题时，不同食物出现的可能性不同。
>
> 当Temperature为0时，模型总会选择其中最有可能的一个，即披萨。
>
> 而当Temperature为0.3时，模型才有可能选择可能性较低其他食物。
>
> 当Temperature为0.7时，模型才有可能选择更低可能性的其他食物。



# NLP研究的领域



| 一级分类 | 二级分类           | 三级分类           |
| -------- | ------------------ | ------------------ |
| 基础研究 | 语法分析           | 中文分词           |
|          |                    | 词性标注           |
|          |                    | 命名实体识别       |
|          | 句法分析           | 结构句法分析       |
|          |                    | 依存句法分析       |
|          |                    | 深层文法句法分析   |
|          | 语篇分析           | 连贯性衔接性分析   |
|          |                    | 语篇结构分析       |
|          |                    | 指代消解           |
|          | 语义分析           | 词义消歧           |
|          |                    | 语义角色标注       |
|          | 语言认知模型       | -                  |
|          | 语言表示与深度学习 | 离散表示           |
|          |                    | 连续表示(深度学习) |
|          | 知识图谱           | 知识表示           |
|          |                    | 知识图谱构建       |
|          |                    | 知识图谱应用       |
| 应用研究 | 文本分类与聚类     | 文本表示           |
|          |                    | 文本分类模型       |
|          |                    | 文本聚类模型       |
|          | 信息提取           | 关系提取           |
|          |                    | 事件提取           |
|          | 情感分析           | 情感分类           |
|          |                    | 情感信息提取       |
|          |                    | 多模态情感分析     |
|          | 自动文摘           | 抽取式摘要         |
|          |                    | 生成式摘要         |
|          | 信息检索           | 信息需求理解       |
|          |                    | 结果匹配排序       |
|          |                    | 信息检索评价       |
|          | 信息推荐与过滤     | 信息推荐           |
|          |                    | 信息过滤           |
|          | 智能问答           | 检索式问答         |
|          |                    | 社区问答           |
|          |                    | 知识库问答         |
|          | 机器翻译           |                    |
|          | 语音技术           |                    |
|          | 文字识别           |                    |
|          | 多模态信息处理     |                    |





## 基础研究的实例

| 二级分类           | 三级分类           | 简要解释                                                 | 实例                                                         | 实例结果                                                     |
| ------------------ | ------------------ | -------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 语法分析           | 中文分词           |                                                          | 给定一个句子：“我喜欢自然语言处理技术”，请进行中文分词。     | 我/喜欢/自然语言/处理/技术                                   |
|                    | 词性标注           | 标注：名词、动词、形容词等                               | 请标注下列词语的词性：今天、去、公园、跑步。                 | 今天（时间词）、去（动词）、公园（名词）、跑步（动词）       |
|                    | 命名实体识别       | 在文本中识别并标注出人名、地名、机构名等有特定意义的实体 | 标注这句话中的命名实体：“李华在北京大学读书。”               | 李华（人名）、北京大学（机构名）                             |
| 句法分析           | 结构句法分析       | 分析句子的结构成分和它们之间的层次关系                   | 请分析以下句子的句法结构：The cat sat on the mat             | 主语：The cat 谓语：sat 宾语：the mat                        |
|                    | 依存句法分析       | 揭示词语间的依赖关系，形成依存句法树。                   | 请进行依存句法分析：The cat sat on the mat                   | cat -> sat, sat -> on, on -> mat                             |
|                    | 深层文法句法分析   | 运用深层文法理论对句子进行更复杂的结构化分析             | 请进行深层文法句法分析：The boy who was chasing the ball kicked it | [NP [NNS The boy] [SBAR [WHNP who] [S [VP was chasing [NP the ball]]]]] [VP kicked [NP it]] |
| 语篇分析           | 连贯性衔接性分析   | 分析篇章内部各句子或段落如何通过连贯性和衔接手段相互联系 | 请分析以下文本的连贯性和衔接性：他喜欢吃苹果。因为他认为苹果很有营养。 | 连贯性：因果关系 衔接性：代词“他”和“苹果”                    |
|                    | 语篇结构分析       | 探究文章的整体结构与组织方式                             | 请进行语篇结构分析：在会议上，主席首先做了开场白，然后是各部门的汇报。 | 开场白-各部门汇报                                            |
|                    | 指代消解           | 确定文本中的代词或指示词所指的对象                       | 请进行指代消解：他喜欢吃苹果。因为他认为它很有营养。         | 他 -> 喜欢吃苹果的人，它 -> 苹果                             |
| 语义分析           | 词义消歧           | 确定多义词在上下文中的具体意义                           | 请解释在句子'他拿着苹果'中'苹果'的意思。                     | 水果苹果                                                     |
|                    | 语义角色标注       | 标注句子中每个成分的语义角色                             | 请进行语义角色标注：The agent gave the patient to the recipient | agent: The agent, patient: the patient, recipient: the recipient |
| 语言认知模型       | -                  | 模拟人类语言理解和生成的认知过程                         | 请根据语言认知模型解释儿童如何学习语言                       | 儿童通过模仿、反馈和大脑发育学习语言                         |
| 语言表示与深度学习 | 离散表示           | 使用离散符号表示语言                                     | 请用独热编码表示单词'cat'                                    | [0, 0, 1, 0, 0, ...]                                         |
|                    | 连续表示(深度学习) | 使用连续向量表示语言                                     | 请用词嵌入表示单词'cat'                                      | [0.2, -0.1, 0.5, ...]                                        |
| 知识图谱           | 知识表示           | 将知识编码为图结构                                       | 请用知识图谱表示'苹果是一种水果'                             | (苹果) -[是]->_[种类]-(水果)                                 |
|                    | 知识图谱构建       | 从数据中构建知识图谱                                     | 请构建一个关于水果的知识图谱                                 | 根据数据源构建的知识图谱                                     |
|                    | 知识图谱应用       | 使用知识图谱解决问题                                     | 请使用知识图谱回答'苹果是什么颜色的?'                        | 根据知识图谱中的信息回答                                     |



## 应用研究的实例



| 二级分类       | 三级分类       | 简要解释                                                     | ChatGPT的Prompt实例                                          | 输出结果                                                     |
| -------------- | -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 文本分类与聚类 | 文本表示       | 将文本转换为计算机可以理解和处理的形式，如词向量、句向量等。 | 将以下文本转换为词向量：'我喜欢吃苹果。'                     | [0.1, 0.2, 0.3, 0.4, 0.5]                                    |
|                | 文本分类模型   | 使用机器学习模型对文本进行分类，如垃圾邮件分类、情感分类等。 | 请对以下邮件进行垃圾邮件分类：'亲爱的用户，恭喜您获得了一等奖，请点击链接领取。'<br/>对以下文本进行分类：'全球变暖正在加剧'。 | 垃圾邮件<br/>类别：环境科学                                  |
|                | 文本聚类模型   | 使用机器学习模型对文本进行聚类，如新闻聚类、社交媒体聚类等。 | 对以下文本进行聚类：['苹果发布新产品', '全球气候变化', '股市今日大跌'] | 聚类结果：[科技, 环境, 经济]                                 |
| 信息提取       | 关系提取       | 从文本中提取实体之间的关系，如提取电影中的人物关系。         | 请提取以下句子中的关系：'Tom和Jerry是好朋友。'               | Tom和Jerry之间的关系：好朋友                                 |
|                | 事件提取       | 从文本中提取事件信息，如提取新闻报道中的事件。               | 请提取以下新闻报道中的事件：'地震导致数十人死亡。'<br/>从以下句子中提取事件：'2020年东京奥运会延期至2021年。' | 地震导致数十人死亡<br/>事件：奥运会延期，时间：2020年至2021年，地点：东京 |
| 情感分析       | 情感分类       | 对文本进行情感分类，如正面、负面、中性等。                   | "请对以下评论进行情感分类：'这部电影太棒了！'"               | 正面情感                                                     |
|                | 情感信息提取   | 从文本中提取情感信息，如提取评论中的情感词汇。               | 请提取以下评论中的情感词汇：'这部电影太棒了！'<br/>分析以下评论的情感倾向：'这家餐厅的食物真糟糕。 | 情感词汇：棒<br/>情感倾向：负面                              |
|                | 多模态情感分析 | 结合文本、图像、声音等多种模态进行情感分析。                 | 分析以下文本和图片的情感倾向：'这是我最开心的一天！' [图片：微笑的人群] | 情感倾向：正面                                               |
| 自动文摘       | 抽取式摘要     | 从文本中抽取关键信息，生成摘要。                             | 请对以下新闻报道进行抽取式摘要：'地震导致数十人死亡。'       | 地震导致数十人死亡                                           |
|                | 生成式摘要     | 使用机器学习模型对文本进行生成式摘要。                       | 请对以下新闻报道进行生成式摘要：'地震导致数十人死亡。'       | 地震导致数十人死亡的报道                                     |
| 信息检索       | 信息需求理解   | 理解用户的信息需求，如查询意图、关键词等。                   | 请理解以下用户查询意图：'我想了解苹果公司的最新动态。'<br/>理解以下搜索查询的意图：'找到处理失眠的最好方法。 | 用户查询意图：了解苹果公司的最新动态<br/>意图：寻找治疗失眠的方法 |
|                | 结果匹配排序   | 根据用户的信息需求，对检索结果进行匹配和排序。               | "请对以下检索结果进行匹配和排序：'苹果公司最新动态'，检索结果：[苹果公司发布新产品，苹果公司股价上涨，苹果公司裁员。]" | 检索结果排序：[苹果公司发布新产品，苹果公司股价上涨，苹果公司裁员] |
|                | 信息检索评价   | 对信息检索系统的效果进行评价，如准确率、召回率等。           | 请对以下信息检索系统的效果进行评价：准确率、召回率。         | 信息检索系统效果评价：准确率0.8，召回率0.9                   |
| 信息推荐与过滤 | 信息推荐       | 根据用户的历史行为和兴趣，向用户推荐相关信息。               | 请对以下用户进行信息推荐：用户历史行为：[苹果公司，科技新闻]，用户兴趣：[科技，创新。] | 信息推荐结果：[苹果公司最新动态，科技新闻报道]               |
|                | 信息过滤       | 根据用户的需求和偏好，对信息进行过滤。                       | 从新闻流中过滤掉关于体育的内容。                             | 结果：[除体育外的新闻内容]                                   |
| 智能问答       | 检索式问答     | 使用检索技术对用户的提问进行回答。                           | "请对以下用户提问进行检索式回答：'苹果公司最近有什么新闻？'" | 苹果公司最近发布了新产品。                                   |
|                | 社区问答       | 在社区中搜索用户的提问和回答。                               | "请对以下用户提问进行社区问答：'如何提高英语口语水平？'"     | 提高英语口语水平的建议：多听多说，参加英语角，模仿英语电影等 |
|                | 知识库问答     | 使用特定知识库回答问题。                                     | 使用Wikipedia回答问题：'介绍达尔文的进化论。'                | 答案：[达尔文进化论的概述]                                   |
| 机器翻译       |                | 自动将一种语言翻译成另一种语言。                             | 将以下句子翻译成英文：'这是一个革命性的科技发展。'           | "翻译结果：'This is a revolutionary technological development.' |
| 语音技术       |                | 识别、理解和生成语音信息。                                   | 将以下文字转换为语音：'欢迎使用我们的服务。'                 | 语音输出：[音频文件播放'欢迎使用我们的服务。']               |
| 文字识别       |                | 从图像中识别和理解文字内容。                                 | 识别以下图片中的文字：[含文字的图片]                         | 识别结果：'未来属于那些准备好的人。'                         |
| 多模态信息处理 |                | 结合和处理多种类型的数据（如文本、图像、声音）。             | 结合图像和文本信息回答问题：'这是哪种植物？' [图片：一种植物] | 答案：这是一棵橡树。                                         |



