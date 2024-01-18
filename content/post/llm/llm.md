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



| 研究领域             | 例子                                             | 使用GPT的提示词                                              |
| -------------------- | ------------------------------------------------ | ------------------------------------------------------------ |
| 机器翻译             | 将英语文本翻译成中文，例如谷歌翻译               | “Translate the following English text to Chinese: [英文文本]” |
| 情感分析             | 从产品评论中识别消费者的正面或负面情绪           | “Analyze the sentiment of the following review: [评论文本]”  |
| 文本分类             | 将新闻文章分类为体育、政治、娱乐等类别           | “Classify the following news article into categories like sports, politics, entertainment: [新闻文章]” |
| 语音识别             | 将语音命令转换成文本，如Apple的Siri              | （GPT不直接处理音频，但可以将转录后的文本进行处理）“Convert the following spoken words into text: [转录文本]” |
| 语音合成             | 从文本生成语音，如Amazon的Alexa                  | （GPT不直接生成音频，但可以用于编写合成语音的脚本）“Write a script for a voice assistant to say: [文本]” |
| 自然语言理解         | 理解和回应用户查询，如IBM的Watson                | “Respond to the following user query: [用户查询]”            |
| 信息检索             | 通过关键词搜索从大量文档中检索信息，如Google搜索 | “Retrieve information on the topic: [关键词]”                |
| 信息抽取             | 从新闻文章中提取关键事件和人物                   | “Extract key events and figures from the following news article: [新闻文章]” |
| 问答系统             | 回答特定问题，如Quora或Stack Overflow            | “Answer the following question: [问题]”                      |
| 对话系统和聊天机器人 | 自动与用户进行交流，如客服聊天机器人             | “Conduct a conversation with a user asking about [主题]”     |
| 文本生成             | 自动生成新闻报道或故事                           | “Generate a news report/story about: [主题]”                 |
| 文本摘要             | 自动生成文章或报告的摘要                         | “Summarize the following article/report: [文章/报告]”        |
| 语言建模             | 预测下一个单词或短语的模型，如GPT系列            | “Complete the following sentence: [句子开头]”                |
| 命名实体识别         | 识别文本中的人名、地点和组织名称                 | “Identify the named entities in the following text: [文本]”  |
| 依存解析和句法分析   | 分析句子结构，识别主语、宾语等                   | “Analyze the sentence structure of the following sentence: [句子]” |
| 语言资源构建         | 创建用于训练机器学习模型的语料库                 | “Compile a corpus of text on the topic: [主题]”              |
| 跨语言NLP            | 处理多种语言的翻译和信息提取                     | “Translate and extract information from the following text in [语言]: [文本]” |
| 生物和医学NLP        | 从医学文献中提取特定疾病或治疗信息               | “Extract information about [疾病/治疗] from the following medical text: [医学文本]” |







# OpenAI官方Prompt例子



## 提取

| Title                      | Description                                                  | 作用                                         |
| -------------------------- | ------------------------------------------------------------ | -------------------------------------------- |
| Summarize for a 2nd grader | Simplify text to a level appropriate for a second-grade student. | 将艰深的概念转换成更容易听明白的解释（高级） |
| Parse unstructured data    | Create tables from unstructured text.                        | 将凌乱的文本转成整齐的文案                   |
| Explain code               | Explain a complicated piece of code.                         | 解释代码                                     |
| Keywords                   | Extract keywords from a block of text.                       | 获取关键字                                   |
| Tweet classifier           | Detect sentiment in a tweet.                                 | 情感分类                                     |
| Mood to color              | Turn a text description into a color.                        | 情绪转颜色（高级）                           |
| Meeting notes summarizer   | Summarize meeting notes including overall discussion, action items, and future topics. | 总结会议记录                                 |
| Review classifier          | Classify user reviews based on a set of tags.                | 分类评论                                     |

**Review classifier**

这个挺有图的，将两个可选值放在一起，让LLM来选择。



> SYSTEM

You will be presented with user reviews and your job is to provide a set of tags from the following list. Provide your answer in bullet point form. Choose ONLY from the list of tags provided here (choose either the positive or the negative tag but NOT both):
    - Provides good value for the price OR Costs too much
    - Works better than expected OR Did not work as well as expected
    - Includes essential features OR Lacks essential features
    - Easy to use OR Difficult to use
    - High quality and durability OR Poor quality and durability
    - Easy and affordable to maintain or repair OR Difficult or costly to maintain or repair
    - Easy to transport OR Difficult to transport
    - Easy to store OR Difficult to store
    - Compatible with other devices or systems OR Not compatible with other devices or systems
    - Safe and user-friendly OR Unsafe or hazardous to use
    - Excellent customer support OR Poor customer support
    - Generous and comprehensive warranty OR Limited or insufficient warranty

> USER

I recently purchased the Inflatotron 2000 airbed for a camping trip and wanted to share my experience with others. Overall, I found the airbed to be a mixed bag with some positives and negatives.        Starting with the positives, the Inflatotron 2000 is incredibly easy to set up and inflate. It comes with a built-in electric pump that quickly inflates the bed within a few minutes, which is a huge plus for anyone who wants to avoid the hassle of manually pumping up their airbed. The bed is also quite comfortable to sleep on and offers decent support for your back, which is a major plus if you have any issues with back pain.        On the other hand, I did experience some negatives with the Inflatotron 2000. Firstly, I found that the airbed is not very durable and punctures easily. During my camping trip, the bed got punctured by a stray twig that had fallen on it, which was quite frustrating. Secondly, I noticed that the airbed tends to lose air overnight, which meant that I had to constantly re-inflate it every morning. This was a bit annoying as it disrupted my sleep and made me feel less rested in the morning.        Another negative point is that the Inflatotron 2000 is quite heavy and bulky, which makes it difficult to transport and store. If you're planning on using this airbed for camping or other outdoor activities, you'll need to have a large enough vehicle to transport it and a decent amount of storage space to store it when not in use. 



**Mood to color**

> SYSTEM

You will be provided with a description of a mood, and your task is to generate the CSS code for a color that matches it. Write your output in json with a single key called "css_code".

> USER

Blue sky at dusk.



**Tweet classifier**

> SYSTEM

You will be provided with a tweet, and your task is to classify its sentiment as positive, neutral, or negative.

> USER

I loved the new Batman movie!



**Summarize for a 2nd grader**

> SYSTEM

Summarize content you are provided with for a second-grade student.

> USER

Jupiter is the fifth planet from the Sun and the largest in the Solar System. It is a gas giant with a mass one-thousandth that of the Sun, but two-and-a-half times that of all the other planets in the Solar System combined. Jupiter is one of the brightest objects visible to the naked eye in the night sky, and has been known to ancient civilizations since before recorded history. It is named after the Roman god Jupiter.[19] When viewed from Earth, Jupiter can be bright enough for its reflected light to cast visible shadows,[20] and is on average the third-brightest natural object in the night sky after the Moon and Venus.





**Parse unstructured data**

> SYSTEM

You will be provided with unstructured data, and your task is to parse it into CSV format.

> USER

There are many fruits that were found on the recently discovered planet Goocrux. There are neoskizzles that grow there, which are purple and taste like candy. There are also loheckles, which are a grayish blue fruit and are very tart, a little bit like a lemon. Pounits are a bright green color and are more savory than sweet. There are also plenty of loopnovas which are a neon pink flavor and taste like cotton candy. Finally, there are fruits called glowls, which have a very sour and bitter taste which is acidic and caustic, and a pale orange tinge to them.



**Keywords**

> SYSTEM

You will be provided with a block of text, and your task is to extract a list of keywords from it.

>USER

Black-on-black ware is a 20th- and 21st-century pottery tradition developed by the Puebloan Native American ceramic artists in Northern New Mexico. Traditional reduction-fired blackware has been made for centuries by pueblo artists. Black-on-black ware of the past century is produced with a smooth surface, with the designs applied through selective burnishing or the application of refractory slip. Another style involves carving or incising designs and selectively polishing the raised areas. For generations several families from Kha'po Owingeh and P'ohwhóge Owingeh pueblos have been making black-on-black ware with the techniques passed down from matriarch potters. Artists from other pueblos have also produced black-on-black ware. Several contemporary artists have created works honoring the pottery of their ancestors.





## 生成



## 转换



| itle                      | Description                                               |      |
| ------------------------- | --------------------------------------------------------- | ---- |
| Grammar Correction        | Convert ungrammatical statements into standard English.   | Y    |
| Emoji Translation         | Translate regular text into emoji text.                   |      |
| Calculate Time Complexity | Find the time complexity of a function.                   |      |
| Python Bug Fixer          | Find and fix bugs in source code.                         |      |
| Airport Code Extractor    | Extract airport codes from text.                          |      |
| Turn by Turn Directions   | Convert natural language to turn-by-turn directions.      |      |
| Improve Code Efficiency   | Provide ideas for efficiency improvements to Python code. |      |
| Memo Writer               | Generate a company memo based on provided points.         |      |
| Translation               | Translate natural language text.                          |      |



**Grammar Correction**

```
You will be provided with statements, and your task is to convert them to standard English.
----
She no went to the market.
```

输出

```
She did not go to the market.
```



Emoji

```
You will be provided with text, and your task is to translate it into emojis. Do not use any regular text. Do your best with emojis only.
------
Artificial intelligence is a technology with great promise.
```

输出

```
🤖💡🔮✨🚀
```

