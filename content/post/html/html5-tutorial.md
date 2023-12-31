---
title: "HTML 常见元素"
description: 
date: 2023-12-18T13:34:46+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false

---



# 常见元素



### 文档结构

| 标签              | 说明                     |
| ----------------- | ------------------------ |
| `<!DOCTYPE html>` | 定义文档类型和 HTML 版本 |
| `<html>`          | HTML 页面的根元素        |
| `<head>`          | 包含文档的元信息         |
| `<body>`          | 包含可见的页面内容       |
| `<title>`         | 定义文档的标题           |



### 内容分区

| 标签        | 说明                           |
| ----------- | ------------------------------ |
| `<header>`  | 代表引导内容或一组导航链接     |
| `<nav>`     | 定义导航链接                   |
| `<section>` | 定义文档中的一个区段           |
| `<article>` | 定义独立的、自成一体的内容     |
| `<aside>`   | 定义放置于其所在内容之外的内容 |
| `<footer>`  | 为文档或区段定义页脚           |
| `<main>`    | 指定文档的主要内容             |



### 文本内容

| 标签           | 说明                          |
| -------------- | ----------------------------- |
| `<h1>`-`<h6>`  | HTML 标题                     |
| `<p>`          | 定义段落                      |
| `<hr>`         | 代表主题性的断开              |
| `<pre>`        | 定义预格式化的文本            |
| `<blockquote>` | 定义从另一来源引用的区段      |
| `<ol>`         | 有序列表                      |
| `<ul>`         | 无序列表                      |
| `<li>`         | 列表项                        |
| `<dl>`         | 描述列表                      |
| `<dt>`         | 在描述列表中定义术语/名称     |
| `<dd>`         | 在描述列表中描述一个术语/名称 |



### 行内文本语义

| 标签       | 说明                 |
| ---------- | -------------------- |
| `<a>`      | 定义超链接           |
| `<span>`   | 一个通用的行内容器   |
| `<strong>` | 定义重要的文本       |
| `<em>`     | 定义强调文本         |
| `<mark>`   | 突出显示文本         |
| `<small>`  | 定义较小的文本       |
| `<cite>`   | 定义作品的标题       |
| `<q>`      | 定义短引用           |
| `<abbr>`   | 定义缩写或首字母缩写 |
| `<time>`   | 代表特定的时间段     |



### 表单和输入

| 标签         | 说明                     |
| ------------ | ------------------------ |
| `<form>`     | 定义用户输入的 HTML 表单 |
| `<input>`    | 定义输入控件             |
| `<textarea>` | 定义多行输入控件         |
| `<button>`   | 定义可点击的             |



### 嵌入式内容

| 标签       | 说明                         |
| ---------- | ---------------------------- |
| `<img>`    | 表示一个图像                 |
| `<iframe>` | 代表嵌套的浏览上下文         |
| `<embed>`  | 定义一个外部应用程序的容器   |
| `<object>` | 定义嵌入的对象               |
| `<video>`  | 嵌入支持视频播放的媒体播放器 |
| `<audio>`  | 嵌入支持音频播放的媒体播放器 |
| `<source>` | 为媒体元素指定多个媒体资源   |



### 脚本

| 标签         | 说明                                 |
| ------------ | ------------------------------------ |
| `<script>`   | 定义客户端脚本                       |
| `<noscript>` | 为不支持客户端脚本的用户定义替代内容 |
| `<canvas>`   | 用于即时绘制图形                     |



# form

https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/button



# 参考

这个不错，可以显示各种元素的样式

https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a



# lang属性

[参考](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/lang)

```html
<p>This paragraph is English, but the language is not specifically defined.</p>

<p lang="en-GB">This paragraph is defined as British English.</p>

<p lang="fr">Ce paragraphe est défini en français.</p>
```



```css
p::before {
  padding-right: 5px;
}

[lang='en-GB']::before {
  content: '(In British English) ';
}

[lang='fr']::before {
  content: '(In French) ';
}
```



使用[BCP-47标准](https://datatracker.ietf.org/doc/html/rfc5646)



## 中文

> zh 现在不是语言code了，而是macrolang
>
> 能作为语言code的是cmn（国语）、yue（粤语）、wuu（吴语）等

```bash
1. 简体中文页面：html lang=zh-cmn-Hans
2. 繁体中文页面：html lang=zh-cmn-Hant
3. 英语页面：html lang=en
```

[参考](https://www.zhihu.com/question/20797118)
