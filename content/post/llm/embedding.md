---
title: "Embedding"
description: 
date: 2024-01-25T14:52:50+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# Embedding模型

常见的模型：

- Word2Vec：一种基于神经网络的模型，用于生成单词的向量表示。
- GloVe：一种基于共现矩阵的模型，用于生成单词的向量表示。
- FastText：Facebook开发的一种基于字符级别的文本嵌入模型，可以为单词和短语生成向量表示。
- BERT：一种基于Transformer的语言模型，可以生成单词、短语甚至是整个句子的向量表示。

- **BGE**：这是国人开发的中文embedding模型，在HuggingFace的MTEB（海量文本Embedding基准）上排名前2，实力强劲。
- **M3E**：Moka Massive Mixed Embedding，也是国人开发的中文embedding模型，我们之前用的就是这个模型，总体来说也算可以，这个还看大家的使用场景，也许你的场景会比我们更加适用。
- **通义千问的embedding模型**：因为是1500+维的模型，所以我们在国庆节后准备用用看。
- **Text-embedding-ada-002**：这是OpenAI的embedding模型，1536维，我感觉上应该是目前最好的模型，但是它在MTEB上排名好像只有第六，但是国内应该也不太能用，所以我们就放弃了。





# 中文：智源的BGE模型

> BGE（BAAI General Embedding）

FlagEmbedding：[Github](https://github.com/FlagOpen/FlagEmbedding)

在中英文语义检索精度与整体语义表征能力均超越了社区所有同类模型，如OpenAI 的text embedding 002等。此外，BGE 保持了同等参数量级模型中的最小向量维度，使用成本更低。

BGE模型将任意文本映射为低维稠密向量，以用于检索、分类、聚类或语义匹配等任务，并可支持为大模型调用外部知识。

[参考](https://www.tizi365.com/topic/10092.html)



# MTEB榜单

> Massive Text Embedding BenchMark（[链接](https://huggingface.co/spaces/mteb/leaderboard?spm=a2c6h.12873639.article-detail.9.93e62bb5aKKpaZ)）

