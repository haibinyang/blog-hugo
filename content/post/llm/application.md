---
title: "LLM应用场景"
description: 
date: 2024-01-30T10:54:06+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---





# OpenAI Prompt官方实例

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



| itle                      | Description                                               |                    |
| ------------------------- | --------------------------------------------------------- | ------------------ |
| Grammar Correction        | Convert ungrammatical statements into standard English.   | Y                  |
| Emoji Translation         | Translate regular text into emoji text.                   |                    |
| Calculate Time Complexity | Find the time complexity of a function.                   |                    |
| Python Bug Fixer          | Find and fix bugs in source code.                         |                    |
| Airport Code Extractor    | Extract airport codes from text.                          | 获取机场代码       |
| Turn by Turn Directions   | Convert natural language to turn-by-turn directions.      | 转成有一步步的指引 |
| Improve Code Efficiency   | Provide ideas for efficiency improvements to Python code. |                    |
| Memo Writer               | Generate a company memo based on provided points.         |                    |
| Translation               | Translate natural language text.                          |                    |



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



**Emoji**

```
You will be provided with text, and your task is to translate it into emojis. Do not use any regular text. Do your best with emojis only.
------
Artificial intelligence is a technology with great promise.
```

输出

```
🤖💡🔮✨🚀
```



**Airport Code Extractor**

```
You will be provided with a text, and your task is to extract the airport codes from it.
----
I want to fly from Orlando to Boston
```

输出

```
Based on your text, the airport codes you are looking for are:

Orlando: MCO (Orlando International Airport)
Boston: BOS (Logan International Airport)
```



**Turn by turn directions**

```
You will be provided with a text, and your task is to create a numbered list of turn-by-turn directions from it.
----
Go south on 95 until you hit Sunrise boulevard then take it east to us 1 and head south. Tom Jenkins bbq will be on the left after several miles.
```

输出

```
Sure, here's a numbered list of turn-by-turn directions based on the provided text:

Start by going south on Interstate 95 (I-95).
Continue on I-95 until you reach Sunrise Boulevard.
Exit onto Sunrise Boulevard and take it eastward.
Follow Sunrise Boulevard to US Route 1 (US 1).
Once you reach US 1, turn and head south.
Continue south on US 1 for several miles.
Look for Tom Jenkins BBQ on the left side of the road.
```



**Generate a company memo based on provided points**

```
Draft a company memo to be distributed to all employees. The memo should cover the following specific points without deviating from the topics mentioned and not writing any fact which is not present here:
    
    Introduction: Remind employees about the upcoming quarterly review scheduled for the last week of April.
    
    Performance Metrics: Clearly state the three key performance indicators (KPIs) that will be assessed during the review: sales targets, customer satisfaction (measured by net promoter score), and process efficiency (measured by average project completion time).
    
    Project Updates: Provide a brief update on the status of the three ongoing company projects:
    
    a. Project Alpha: 75% complete, expected completion by May 30th.
    b. Project Beta: 50% complete, expected completion by June 15th.
    c. Project Gamma: 30% complete, expected completion by July 31st.
    
    Team Recognition: Announce that the Sales Team was the top-performing team of the past quarter and congratulate them for achieving 120% of their target.
    
    Training Opportunities: Inform employees about the upcoming training workshops that will be held in May, including "Advanced Customer Service" on May 10th and "Project Management Essentials" on May 25th.
```



## 按NL研究领域：分类



| 二级分类       | 三级分类       |                                                              |
| -------------- | -------------- | ------------------------------------------------------------ |
| 文本分类与聚类 | 文本表示       |                                                              |
|                | 文本分类模型   | Review classifier<br/>Pro and con discusser                  |
|                | 文本聚类模型   |                                                              |
| 信息提取       | 关系提取       | Parse unstructured data<br/>Keywords<br/>Spreadsheet creator<br/>Airport code extractor<br/>Natural language to SQL |
|                | 事件提取       |                                                              |
| 情感分析       | 情感分类       | Tweet classifier                                             |
|                | 情感信息提取   |                                                              |
|                | 多模态情感分析 |                                                              |
| 自动文摘       | 抽取式摘要     | Meeting notes summarizer                                     |
|                | 生成式摘要     | Summarize for a 2nd grader<br/>Product name generator<br/>VR fitness idea generator<br/>Interview questions<br/>Rap battle writer<br/>Memo writer<br/>Lesson plan writer |
| 信息检索       | 信息需求理解   |                                                              |
|                | 结果匹配排序   |                                                              |
|                | 信息检索评价   |                                                              |
| 信息推荐与过滤 | 信息推荐       |                                                              |
|                | 信息过滤       |                                                              |
| 智能问答       | 检索式问答     |                                                              |
|                | 社区问答       |                                                              |
|                | 知识库问答     |                                                              |
| 机器翻译       |                | Emoji Translation<br/>Turn by turn directions<br/>Translation |
| 语音技术       |                |                                                              |
| 文字识别       |                |                                                              |
| 多模态信息处理 |                |                                                              |



| title                       | description                                                  | 二级分类       | 三级分类     |
| --------------------------- | ------------------------------------------------------------ | -------------- | ------------ |
| Grammar correction          | Convert ungrammatical statements into standard English.      |                |              |
| Summarize for a 2nd grader  | Simplify text to a level appropriate for a second-grade student. | 自动文摘       | 生成式摘要   |
| Parse unstructured data     | Create tables from unstructured text.                        | 信息提取       | 关系提取     |
| Emoji Translation           | Translate regular text into emoji text.                      | 机器翻译       | -            |
| Calculate time complexity   | Find the time complexity of a function.                      |                |              |
| Explain code                | Explain a complicated piece of code.                         |                |              |
| Keywords                    | Extract keywords from a block of text.                       | 信息提取       | 关系提取     |
| Product name generator      | Generate product names from a description and seed words.    | 自动文摘       | 生成式摘要   |
| Python bug fixer            | Find and fix bugs in source code.                            |                |              |
| Spreadsheet creator         | Create spreadsheets of various kinds of data.                | 信息提取       | 关系提取     |
| Tweet classifier            | Detect sentiment in a tweet.                                 | 情感分析       | 情感分类     |
| Airport code extractor      | Extract airport codes from text.                             | 信息提取       | 关系提取     |
| Mood to color               | Turn a text description into a color.                        |                |              |
| VR fitness idea generator   | Generate ideas for fitness promoting virtual reality games.  | 自动文摘       | 生成式摘要   |
| Marv the sarcastic chat bot | A factual chatbot that is also sarcastic.                    | 智能对话       | 社区问答     |
| Turn by turn directions     | Convert natural language to turn-by-turn directions.         | 机器翻译       | -            |
| Interview questions         | Create interview questions.                                  | 自动文摘       | 生成式摘要   |
| Function from specification | Create a Python function from a specification.               |                |              |
| Improve code efficiency     | Provide ideas for efficiency improvements to Python code.    |                |              |
| Single page website creator | Create a single page website.                                |                |              |
| Rap battle writer           | Generate a rap battle between two characters.                | 自动文摘       | 生成式摘要   |
| Memo writer                 | Generate a company memo based on provided points.            | 自动文摘       | 生成式摘要   |
| Emoji chatbot               | Generate conversational replies using emojis only.           | 智能问答       | 社区问答     |
| Translation                 | Translate natural language text.                             | 机器翻译       | -            |
| Socratic tutor              | Generate responses as a Socratic tutor.                      | 智能问答       | 社区问答     |
| Natural language to SQL     | Convert natural language into SQL queries.                   | 信息提取       | 关系提取     |
| Meeting notes summarizer    | Summarize meeting notes including overall discussion, action items, and future topics. | 自动文摘       | 抽取式摘要   |
| Review classifier           | Classify user reviews based on a set of tags.                | 文本分类与聚类 | 文本分类     |
| Pro and con discusser       | Analyze the pros and cons of a given topic.                  | 文本分类与聚类 | 文本分类模型 |
| Lesson plan writer          | Generate a lesson plan for a specific topic.                 | 自动文摘       | 生成式摘要   |



## 我的分类





| 类别          | 应用场景                                                     |
| ------------- | ------------------------------------------------------------ |
| 分类          | Review classifier<br/>Pro and con discusser                  |
| 情感分析      | Tweet classifier                                             |
| 提取          | Keywords<br/>Parse unstructured data<br/>Spreadsheet creator<br/>Airport code extractor<br/>Turn by turn directions<br/> |
| 摘要/总结     | Meeting notes summarizer<br/>Summarize for a 2nd grader<br/>解释代码 |
| 生成/写作     | Product name generator<br/>VR fitness idea generator<br/>Interview questions<br/>Rap battle writer<br/>Memo writer<br/>Lesson plan writer |
| 转换/机器翻译 | Translation<br/>Emoji Translation<br/>Mood to color<br/>Natural language to SQL |
| 转换/校对     | Grammar Correction<br/>Python bug fixer                      |
| 代码          |                                                              |
| 知识库        |                                                              |
| 语音合成      |                                                              |


