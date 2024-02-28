---
·title: "RAG:检索增强生成"
description: 
date: 2024-01-25T11:24:38+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---

# RAG

> RAG: Retrieval Augmented Generation

你正试图破解一个复杂的案件：

- 侦探的角色是收集与案件相关的线索、证据和一些历史记录。
- 侦探收集完这些信息后，记者将这些事实总结成一个引人入胜的故事，并呈现一个连贯的叙述。





# LLM的问题



1. 幻觉：在没有答案的情况下提供虚假信息。
2. LLM使用的是过时的信息，它无法访问其知识截止日期之后最新的、可靠的信息。
3. 此外，LLM 提供的答案没有引用其来源，这意味着其主张无法被用户验证是否准确，也无法完全信赖。这突出表明了在使用人工智能生成的信息时进行独立核查和评估的重要性。

您可以将[大型语言模型](https://aws.amazon.com/what-is/large-language-model/)看作是一个过于热情的新员工，他拒绝随时了解时事，但总是会绝对自信地回答每一个问题。



RAG 是解决其中一些挑战的一种方法。它会重定向 LLM，从权威的、预先确定的知识来源中检索相关信息。组织可以更好地控制生成的文本输出，并且用户可以深入了解 LLM 如何生成响应。



# LLM的流程



![image-20240125161258862](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240125161258862.png)



![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/jumpstart-fm-rag.jpg)



# （不懂）检索增强生成和语义搜索有什么区别？

语义搜索可以提高 RAG 结果，适用于想要在其 LLM 应用程序中添加大量外部知识源的组织。现代企业在各种系统中存储大量信息，例如手册、常见问题、研究报告、客户服务指南和人力资源文档存储库等。上下文检索在规模上具有挑战性，因此会降低生成输出质量。

语义搜索技术可以扫描包含不同信息的大型数据库，并更准确地检索数据。例如，他们可以回答诸如 *“去年在机械维修上花了多少钱？”*之类的问题，方法是将问题映射到相关文档并返回特定文本而不是搜索结果。然后，开发人员可以使用该答案为 LLM 提供更多上下文。

RAG 中的传统或关键字搜索解决方案对知识密集型任务产生的结果有限。开发人员在手动准备数据时还必须处理单词嵌入、文档分块和其他复杂问题。相比之下，语义搜索技术可以完成知识库准备的所有工作，因此开发人员不必这样做。它们还生成语义相关的段落和按相关性排序的标记词，以最大限度地提高 RAG 有效载荷的质量。





# RAG的三大核心组件

检索增强生成模型主要由三个核心组件构成：

1. 检索器 (Retriever): 负责从外部知识来源检索相关信息。
2. 排序器 (Ranker): 对检索结果进行评估，并排列优先级。
3. 生成器 (Generator): 利用检索和排序结果，结合用户的输入，生成最终的答案或内容。



# RAG脑图

> [原文](https://mp.weixin.qq.com/s/FqyaTK2Mb4VJolK81P5_1g)

这张图，非常地详细！



![image-20240125161624484](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240125161624484.png)





# 数据索引

- **数据提取**

- - 数据清洗：包括数据Loader，提取PDF、word、markdown以及数据库和API等；
  - 数据处理：包括数据格式处理，不可识别内容的剔除，压缩和格式化等；
  - 元数据提取：提取文件名、时间、章节title、图片alt等信息，非常关键。



# 检索

检索优化一般分为下面五部分工作：

- **元数据过滤**：当我们把索引分成许多chunks的时候，检索效率会成为问题。这时候，如果可以通过元数据先进行过滤，就会大大提升效率和相关度。比如，我们问“帮我整理一下XX部门今年5月份的所有合同中，包含XX设备采购的合同有哪些？”。这时候，如果有元数据，我们就可以去搜索“**XX部门+2023年5月**”的相关数据，检索量一下子就可能变成了全局的万分之一；

- **图关系检索**：如果可以将很多实体变成node，把它们之间的关系变成relation，就可以利用知识之间的关系做更准确的回答。特别是针对一些多跳问题，利用图数据索引会让检索的相关度变得更高；

- **检索技术**：前面说的是一些前置的预处理的方法，检索的主要方式还是这几种：

- - **相似度检索**：前面我已经写过那篇文章《[大模型应用中大部分人真正需要去关心的核心——Embedding](http://mp.weixin.qq.com/s?__biz=MzIyOTA5NTM1OA==&mid=2247484246&idx=1&sn=c0b4f64e829bc336d516b4c79bbda19c&chksm=e846a187df31289169516fdf09ebfc3235dfcc11f467cf9541b022b2f3381a5c464db37f4da6&scene=21#wechat_redirect)》中有提到六种相似度算法，包括欧氏距离、曼哈顿距离、余弦等，后面我还会再专门写一篇这方面的文章，可以关注我，yeah；
  - **关键词检索**：这是很传统的检索方式，但是有时候也很重要。刚才我们说的元数据过滤是一种，还有一种就是先把chunk做摘要，再通过关键词检索找到可能相关的chunk，增加检索效率。据说Claude.ai也是这么做的；
  - **SQL检索**：这就更加传统了，但是对于一些本地化的企业应用来说，SQL查询是必不可少的一步，比如我前面提到的销售数据，就需要先做SQL检索。
  - 其他：检索技术还有很多，后面用到再慢慢说吧。

- **重排序（Rerank）**：很多时候我们的检索结果并不理想，原因是chunks在系统内数量很多，我们检索的维度不一定是最优的，一次检索的结果可能就会在相关度上面没有那么理想。这时候我们需要有一些策略来对检索的结果做重排序，比如使用planB重排序，或者把组合相关度、匹配度等因素做一些重新调整，得到更符合我们业务场景的排序。因为在这一步之后，我们就会把结果送给LLM进行最终处理了，所以这一部分的结果很重要。这里面还会有一个内部的判断器来评审相关度，触发重排序。

- **查询轮换**：这是查询检索的一种方式，一般会有几种方式：

  - **子查询：**可以在不同的场景中使用各种查询策略，比如可以使用LlamaIndex等框架提供的查询器，采用树查询（从叶子结点，一步步查询，合并），采用向量查询，或者最原始的顺序查询chunks等**；**

  - **HyDE：**这是一种抄作业的方式，生成相似的或者更标准的prompt模板**。**





[参考1](https://mp.weixin.qq.com/s/FqyaTK2Mb4VJolK81P5_1g)



## 混合检索

双路查询：

- 语义检索（Vector Search）
- 关键字检索（Keyword Search）



![image-20240204140848656](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240204140848656.png)



[Youtube教程](https://www.youtube.com/watch?v=hsEWohYtg0I&ab_channel=LlamaIndex)



[关键词&语义的混合检索实现](https://mp.weixin.qq.com/s/QptJn4cjd8arEGweW_mEVg)







# 生成

框架有Langchain和LlamaIndex





# 数禾技术的方案



## 框架

难点是：text-to-sql是什么？

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/DEWdgLnJps7wibpbBXkmcibRXZZh2ib9ASNiaJJKmmNFHVHlFTtILbiaibAzl9ZKJFdOx9FbWDdffictoDzwhB75ZcMrA/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&wx_co=1)



## 文本拆分：

文本拆分：将文档分成较小的块，方便后续做文本Embedding，进而方便后续的文档检索。

理想情况：将语义相关的文本片段按照顺序放在一起。



**拆分方法**

- 根据规则：（最简单的方法）按照句子对文档进行拆分。根据中文和英文常见的终止符号来对文档进行分割，例如单字符断句符、中英文省略号、双引号等。
- 根据语义：
  1. 首先基于规则来对文档拆分成句子级别的文档块
  2. 然后使用模型来基于语义对文档块进行整合，最终获得基于语义的文档块



**基于语义的文本拆分模型**

阿里达摩院推出的**SEQ_MODEL**，该模型基于BERT+滑动窗口，通过预测拆分后的句子是否属于段落边界进而确定语义分段。





## 文本向量化：选择Embedding模型

智源的BBA模型（bge-base-zh模型）或者从MTEB榜样中选择。



## 向量存储

Faiss:个人使用

Milvus：生产级别



## 根据提问使用向量检索匹配知识点

top_k

Faiss：在搜索结果的附近进行扩展查找以获得小于chunk_size(一般为500字)的相近文档

Milvus：topk检索+bge-base-zh+段落相似聚合模型



思想：分析基于topk检索的扩展检索思路，我们发现其主要是通过扩展语义段来让大模型在回答的时候获取尽可能多的有用信息来提高回答效果。

思路：

1. 首先基于规则来对文档拆分成句子级别的文档块
2. 然后使用模型来基于语义对文档块进行整合，最终获得基于语义的文档块
3. 再次顺序对文档使用文本embedding模型，按照语义相似度再次聚合，相当于两次用不同的方法对原来句子级别的文档聚合了两次。





## 构建Prompt



```bash
你现在是一个智能助手了，现在需要你根据已知内容回答问题

已知内容如下:

"{context}"



通过对已知内容进行总结并且列举的方式来回答问题:"{question}"，在答案中不能出现问题内容，并且不允许编造内容，并且使用简体中文回答。

如果该问题和已知内容不相关，请回答 "根据已知信息无法回答该问题" 或 "没有提供足够的相关信息"。
```



## 生成答案：选择LLM





## 测试方案



![image-20240125144002221](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240125144002221.png)





[参考](https://mp.weixin.qq.com/s/XITLDMF9him-g-pDt85IqQ)



# RAG的痛点和解决方案

[链接](https://blog.llamaindex.ai/a-cheat-sheet-and-some-recipes-for-building-advanced-rag-803a9d94c41b)

非常不错的文章

还没有时间看！！

![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/rag-cheat-sheet-final.svg)









# AWS中的RAG服务

[Amazon Bedrock](https://aws.amazon.com/bedrock/)

[Amazon Kendra](https://aws.amazon.com/kendra/) 

[参考](https://aws.amazon.com/cn/what-is/retrieval-augmented-generation/)