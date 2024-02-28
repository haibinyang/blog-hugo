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



# 基础和概念

## Prompt

> 中文翻译：提示词

给机器的指令，类似编程语言。



## Token

待定



## LLM

> 中文翻译：大模型

本质：一套算法，类似一个函数。



### 常见的模型

- OpenAI: GPT-3.5/GPT-4/GPT-4V/DALL.EWhisper
- Meta: LLama2（开源）



## GPT模型



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



### 支持的格式

| 格式类型     | 例子                                                |
| ------------ | --------------------------------------------------- |
| 编号列表     | 1. 打开浏览器<br>2. 输入网址<br>3. 浏览内容         |
| 圆点列表     | - 苹果<br>- 香蕉<br>- 橙子                          |
| 表格         |                                                     |
| 代码块       | ```python<br>print("Hello, world!")<br>```          |
| 标题和子标题 | ## 主标题<br>### 子标题                             |
| JSON格式     | `{ "name": "John", "age": 30, "city": "New York" }` |
| 思维导图格式 |                                                     |
| markdown格式 |                                                     |





