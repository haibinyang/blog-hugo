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



### **Review classifier**

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



#### 中文

> Prompt

```bash
你的任务是从以下的【用户评论】提取提供一组标签：
- 从以下的【标签列表】中选择 (选择正面或负面标签之一，但不能同时选择两者)
- 使用列表格式输出

【标签列表】
性价比高 OR 价格过高
超出预期 OR 未达到预期效果
包含基本功能 OR 缺少基本功能
易于使用 OR 使用困难
高质量和耐用 OR 质量差且不耐用
易于且负担得起维护或维修 OR 难以或昂贵维护或维修
易于运输 OR 运输困难
易于存放 OR 存放困难
与其他设备或系统兼容 OR 与其他设备或系统不兼容
安全且用户友好 OR 使用不安全或存在危险
优秀的客户支持 OR 客户支持差
慷慨且全面的保修 OR 保修有限或不足

【用户评论】
最近我购买了 Inflatotron 2000充气床垫用于露营旅行，我想和大家分享一下我的体验。总的来说，我发现这款充气床垫好坏参半，既有优点也有缺点。
首先说说优点吧，Inflatotron 2000 设置和充气都非常方便。 它配有一个内置电动泵，可以在几分钟内快速充气床垫，对于那些不想手动充气床垫的人来说，这是一个巨大的优势。床垫睡起来也很舒服，对背部有很好的支撑，如果你有任何背痛问题，这是一个主要优点。
另一方面，我也遇到了一些 Inflatotron 2000 的缺点。 首先，我发现床垫不是很耐用，容易被刺破。在露营旅行中，床垫被掉落的一根树枝刺破了，这让我很沮丧。其次，我注意到床垫往往会在晚上漏气，这意味着我每天早上都要重新充气。这有点烦人，因为它会影响我的睡眠，让我早上感觉 kurang istirahat。
另一个缺点是 Inflatotron 2000 非常笨重，携带和存放都不方便。 如果您计划将这款充气床垫用于露营或其他户外活动，您需要拥有足够大的车辆来运输它，并且还需要一定的空间来存放它。
```



> 响应

```bash
易于使用
质量差且不耐用
运输困难
存放困难
```



### **Mood to color**

> SYSTEM

You will be provided with a description of a mood, and your task is to generate the CSS code for a color that matches it. Write your output in json with a single key called "css_code".

> USER

Blue sky at dusk.



### **Tweet classifier**

> SYSTEM

You will be provided with a tweet, and your task is to classify its sentiment as positive, neutral, or negative.

> USER

I loved the new Batman movie!



### **Summarize for a 2nd grader**

> SYSTEM

Summarize content you are provided with for a second-grade student.

> USER

Jupiter is the fifth planet from the Sun and the largest in the Solar System. It is a gas giant with a mass one-thousandth that of the Sun, but two-and-a-half times that of all the other planets in the Solar System combined. Jupiter is one of the brightest objects visible to the naked eye in the night sky, and has been known to ancient civilizations since before recorded history. It is named after the Roman god Jupiter.[19] When viewed from Earth, Jupiter can be bright enough for its reflected light to cast visible shadows,[20] and is on average the third-brightest natural object in the night sky after the Moon and Venus.

木星是太阳系中离太阳第五近的行星，也是太阳系中最大的行星。它是一个气体巨星，其质量是太阳的千分之一，但是是太阳系中其他所有行星总质量的两倍半。木星是夜空中肉眼可见的最亮的天体之一，自古以来就为古代文明所知。它的名字来自罗马神话中的木星神。从地球上看，木星足够亮，以至于它反射的光能投下可见的阴影，平均而言，它是继月亮和金星之后夜空中第三亮的自然天体。



### **Parse unstructured data**

> SYSTEM

You will be provided with unstructured data, and your task is to parse it into CSV format.

> USER

There are many fruits that were found on the recently discovered planet Goocrux. There are neoskizzles that grow there, which are purple and taste like candy. There are also loheckles, which are a grayish blue fruit and are very tart, a little bit like a lemon. Pounits are a bright green color and are more savory than sweet. There are also plenty of loopnovas which are a neon pink flavor and taste like cotton candy. Finally, there are fruits called glowls, which have a very sour and bitter taste which is acidic and caustic, and a pale orange tinge to them.



### **Keywords**

> SYSTEM

You will be provided with a block of text, and your task is to extract a list of keywords from it.

>USER

Black-on-black ware is a 20th- and 21st-century pottery tradition developed by the Puebloan Native American ceramic artists in Northern New Mexico. Traditional reduction-fired blackware has been made for centuries by pueblo artists. Black-on-black ware of the past century is produced with a smooth surface, with the designs applied through selective burnishing or the application of refractory slip. Another style involves carving or incising designs and selectively polishing the raised areas. For generations several families from Kha'po Owingeh and P'ohwhóge Owingeh pueblos have been making black-on-black ware with the techniques passed down from matriarch potters. Artists from other pueblos have also produced black-on-black ware. Several contemporary artists have created works honoring the pottery of their ancestors.



### **Meeting notes summarizer**

> SYSTEM

```
You will be provided with meeting notes, and your task is to summarize the meeting as follows:
    
    -Overall summary of discussion
    -Action items (what needs to be done and who is doing it)
    -If applicable, a list of topics that need to be discussed more fully in the next meeting.
```



> USER

```
Meeting Date: March 5th, 2050
    Meeting Time: 2:00 PM
    Location: Conference Room 3B, Intergalactic Headquarters
    
    Attendees:
    - Captain Stardust
    - Dr. Quasar
    - Lady Nebula
    - Sir Supernova
    - Ms. Comet
    
    Meeting called to order by Captain Stardust at 2:05 PM
    
    1. Introductions and welcome to our newest team member, Ms. Comet
    
    2. Discussion of our recent mission to Planet Zog
    - Captain Stardust: "Overall, a success, but communication with the Zogians was difficult. We need to improve our language skills."
    - Dr. Quasar: "Agreed. I'll start working on a Zogian-English dictionary right away."
    - Lady Nebula: "The Zogian food was out of this world, literally! We should consider having a Zogian food night on the ship."
    
    3. Addressing the space pirate issue in Sector 7
    - Sir Supernova: "We need a better strategy for dealing with these pirates. They've already plundered three cargo ships this month."
    - Captain Stardust: "I'll speak with Admiral Starbeam about increasing patrols in that area.
    - Dr. Quasar: "I've been working on a new cloaking technology that could help our ships avoid detection by the pirates. I'll need a few more weeks to finalize the prototype."
    
    4. Review of the annual Intergalactic Bake-Off
    - Lady Nebula: "I'm happy to report that our team placed second in the competition! Our Martian Mud Pie was a big hit!"
    - Ms. Comet: "Let's aim for first place next year. I have a secret recipe for Jupiter Jello that I think could be a winner."
    
    5. Planning for the upcoming charity fundraiser
    - Captain Stardust: "We need some creative ideas for our booth at the Intergalactic Charity Bazaar."
    - Sir Supernova: "How about a 'Dunk the Alien' game? We can have people throw water balloons at a volunteer dressed as an alien."
    - Dr. Quasar: "I can set up a 'Name That Star' trivia game with prizes for the winners."
    - Lady Nebula: "Great ideas, everyone. Let's start gathering the supplies and preparing the games."
    
    6. Upcoming team-building retreat
    - Ms. Comet: "I would like to propose a team-building retreat to the Moon Resort and Spa. It's a great opportunity to bond and relax after our recent missions."
    - Captain Stardust: "Sounds like a fantastic idea. I'll check the budget and see if we can make it happen."
    
    7. Next meeting agenda items
    - Update on the Zogian-English dictionary (Dr. Quasar)
    - Progress report on the cloaking technology (Dr. Quasar)
    - Results of increased patrols in Sector 7 (Captain Stardust)
    - Final preparations for the Intergalactic Charity Bazaar (All)
    
    Meeting adjourned at 3:15 PM. Next meeting scheduled for March 19th, 2050 at 2:00 PM in Conference Room 3B, Intergalactic Headquarters. 
```

> 响应

```
Overall Summary of Discussion:
The team discussed the recent mission to Planet Zog, the space pirate issue in Sector 7, the annual Intergalactic Bake-Off, planning for the upcoming charity fundraiser, and a proposed team-building retreat. The mission to Planet Zog was successful, but communication with the Zogians was challenging. The team also discussed the need for a better strategy to deal with space pirates. The team placed second in the Intergalactic Bake-Off and is planning for the upcoming charity fundraiser. A team-building retreat to the Moon Resort and Spa was proposed.

Action Items:
1. Dr. Quasar to start working on a Zogian-English dictionary.
2. Captain Stardust to speak with Admiral Starbeam about increasing patrols in Sector 7.
3. Dr. Quasar to finalize the prototype of the new cloaking technology.
4. All team members to start gathering supplies and preparing games for the Intergalactic Charity Bazaar.
5. Captain Stardust to check the budget for the proposed team-building retreat to the Moon Resort and Spa.

Topics for Next Meeting:
1. Update on the Zogian-English dictionary.
2. Progress report on the cloaking technology.
3. Results of increased patrols in Sector 7.
4. Final preparations for the Intergalactic Charity Bazaar.
```



```
讨论总摘要：
该团队讨论了最近前往佐格星球的任务、第 7 区的太空海盗问题、一年一度的星际烘焙、即将到来的慈善筹款活动的计划以及拟议的团队建设务虚会。前往佐格星球的任务很成功，但与佐格人的沟通却很有挑战性。该团队还讨论了需要更好的策略来对付太空海盗。该团队在星际烘焙比赛中排名第二，并正在为即将到来的慈善筹款活动做准备。有人提议在月亮度假村和水疗中心进行团队建设。

行动项目：
1. Quasar博士开始编写Zogian-English词典。
2. 星尘上尉与星光海军上将讨论增加 7 区的巡逻事宜。
3. 类星体博士完成新隐身技术的原型。
4. 所有团队成员开始为星际慈善义卖收集物资和准备游戏。
5. 星尘船长检查拟议的月亮度假村和水疗中心团队建设静修会的预算。

下次会议的主题：
1. Zogian-English 词典的更新。
2.隐身技术进展报告。
3. 第 7 区增加巡逻的结果。
4. 星际慈善义卖的最后准备工作。
```



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



## 按NLP研究领域：分类



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



## Deeplearn的分类

- 分类
- 总结
- 推理
- 转换
- 扩写
- 聊天机器人



## 我的分类



| 类别          | 应用场景                                                     | 其它内容其它 |
| ------------- | ------------------------------------------------------------ | ------------ |
| 分类          | [Review classifier](https://platform.openai.com/examples/default-review-classifier) |              |
| 情感分析      | Tweet classifier                                             |              |
| 提取          | Airport code extractor<br/>Parse unstructured data<br/>Turn by turn directions<br/> |              |
| 摘要/总结     | Meeting notes summarizer<br/>Summarize for a 2nd grader<br/>解释代码 |              |
| 扩写/写作     | Product name generator<br/>VR fitness idea generator<br/>Interview questions<br/>Rap battle writer<br/>Memo writer<br/>Lesson plan writer |              |
| 转换/机器翻译 | Translation<br/>Emoji Translation<br/>Mood to color<br/>Natural language to SQL |              |
| 转换/校对     | Grammar Correction<br/>Python bug fixer                      |              |





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



