---
title: "Tailwind Tutorial"
description: 
date: 2023-12-18T13:34:19+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# 集成到Next.js

> [参考](https://tailwindcss.com/docs/guides/nextjs)

创建Next.js项目

```bash
npx create-next-app
```

安装postcss、autoprefixer和tailwindcss库

```bash
npm install -D tailwindcss postcss autoprefixer
```

> 1. **PostCSS**: Tailwind CSS 是基于 PostCSS 构建的，这是一个用于转换 CSS 的工具。它允许你使用 JavaScript 插件来处理 CSS。Tailwind CSS 利用 PostCSS 来实现其功能，如应用实用类和响应式设计。
> 2. **Autoprefixer**: Autoprefixer 是一个 PostCSS 插件，用于自动添加浏览器特定的前缀到 CSS 规则中。这意味着你不需要手动添加这些前缀，Autoprefixer 会根据你的浏览器目标自动处理它们。这对于确保跨浏览器兼容性非常重要。

初始化

```bash
npx tailwindcss init -p
```

生成2个配置文件

- 关键是postcss引进两个插件：tailwind和autoprefixer

![image-20240107085950440](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240107085950440.png)

配置tailwind

```ts
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./app/**/*.{js,ts,jsx,tsx,mdx}",
    "./pages/**/*.{js,ts,jsx,tsx,mdx}",
    "./components/**/*.{js,ts,jsx,tsx,mdx}",
 
    // Or if using `src` directory:
    "./src/**/*.{js,ts,jsx,tsx,mdx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

配置`globals.css`：添加自定义的Directive。

```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

使用tailwind

> index.tsx

```tsx
export default function Home() {
  return (
    <h1 className="text-3xl font-bold underline">
      Hello world!
    </h1>
  )
}
```



## Autoprefixer

Autoprefixer 的作用是自动为 CSS 规则添加浏览器特定的前缀，以确保跨浏览器的兼容性。例如，考虑以下 CSS 规则：

```
cssCopy code
.example {
    display: flex;
    transition: transform 1s;
    user-select: none;
}
```

在不使用 Autoprefixer 的情况下，这些规则可能不会在所有浏览器中正常工作。但是当使用 Autoprefixer 时，它会自动添加必要的前缀，以确保更广泛的浏览器兼容性。处理后的 CSS 可能会看起来像这样：

```css
cssCopy code
.example {
    display: -webkit-box;  /* 旧版 iOS Safari */
    display: -ms-flexbox;  /* IE 10 */
    display: flex;         /* 现代浏览器 */
    -webkit-transition: -webkit-transform 1s;
    transition: -ms-transform 1s; /* IE 9 */
    transition: transform 1s;
    -webkit-user-select: none; /* Safari 和 Chrome */
    -moz-user-select: none;    /* Firefox */
    -ms-user-select: none;     /* IE 10+ 和 Edge */
    user-select: none;         /* 规范语法 */
}
```

这里，Autoprefixer 根据当前的浏览器支持和使用情况，为 `display: flex;`, `transition`, 和 `user-select` 等属性添加了适当的前缀。这样，你的 CSS 就能在更多的浏览器上正常运行，而无需手动添加这些前缀。

## 添加Geist字体

见链接





# 分层

Tailwind CSS 使用多个层（layers）来组织其样式规则。这些层包括：

1. **基础层（Base）**: 这是最底层，包括浏览器样式重置和基础元素样式。例如，它会重置标准的 HTML 元素样式，确保在不同浏览器中具有一致性。
2. **组件层（Components）**: 在这一层，Tailwind 提供了一系列预构建的组件样式。这些样式是针对常见的 UI 元素，如按钮、表单和卡片。通过使用这些组件，开发者可以快速搭建界面而无需从零开始。
3. **实用工具层（Utilities）**: 实用工具层是 Tailwind 的核心特性之一，提供了大量的实用工具类，允许开发者快速应用样式，如边距、颜色、字体大小和布局等。这些类通常是原子性的，意味着每个类只做一件事。
4. **自定义层（Custom）**: 虽然 Tailwind 提供了大量的实用工具类和组件样式，但在某些情况下，开发者可能需要更具体的样式。在这种情况下，可以创建自定义层，添加特定的 CSS 规则来满足特定的需求。

使用这些层，Tailwind CSS 为开发者提供了既灵活又高效的方式来构建和管理样式。通过层的概念，可以轻松地覆盖和扩展样式，确保样式表的可维护性和可扩展性。



# Layout

## Display

[参考](https://tailwindcss.com/docs/display)



| **Class** | **Properties**   |
| --------- | ---------------- |
| block     | display: block;  |
| inline    | display: inline; |
| flex      | display: flex;   |
| table     | display: table;  |
| grid      | display: grid;   |
| hidden    | display: none;   |



### Block

完整占用一行。



![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231220135234.png)

```html
<div class="p-5 bg-slate-100">
  When controlling the flow of text, using the CSS property
  <span class="inline bg-sky-100 font-bold">display: inline</span>
  will cause the text inside the element to wrap normally.

  While using the property <span class="inline-block bg-sky-100 font-bold">display: inline-block</span>
  will wrap the element to prevent the text inside from extending beyond its parent.

  Lastly, using the property <span class="block bg-sky-100 font-bold">display: block</span>
  will put the element on its own line and fill its parent.
</div>
```



### hidden

`display: none`

```html
<div class="flex ...">
  <div class="hidden">01</div>
  <div>02</div>
  <div>03</div>
</div>
```



类似但容易混淆的`invisible`



## Visibility



| Class     | Properties            |
| --------- | --------------------- |
| visible   | visibility: visible;  |
| invisible | visibility: hidden;   |
| collapse  | visibility: collapse; |





# Flexbox

参考[Tailwind官网](https://tailwindcss.com/docs/flex-direction)



## Flex Direction

| Class            | Properties                      |
| ---------------- | ------------------------------- |
| flex-row         | flex-direction: row;            |
| flex-row-reverse | flex-direction: row-reverse;    |
| flex-col         | flex-direction: column;         |
| flex-col-reverse | flex-direction: column-reverse; |



## Flex Wrap

| Class             | Properties               |
| ----------------- | ------------------------ |
| flex-wrap         | flex-wrap: wrap;         |
| flex-wrap-reverse | flex-wrap: wrap-reverse; |
| flex-nowrap       | flex-wrap: nowrap;       |

## Align



| HTML            | Tailwin CSS     | Tailwin CSS |
| --------------- | --------------- | ----------- |
| flex-direction  |                 |             |
| flex-wrap       |                 |             |
| flex-flow       |                 |             |
| justify-content | Justify Content | justify-xxx |
| align-items     | Align Item      | items-xxx   |
| align-content   | Align Content   | content-xxx |



### Justify Content

> Main axis（水平方向）



| Class           | Properties                      |
| --------------- | ------------------------------- |
| justify-normal  | justify-content: normal;        |
| justify-start   | justify-content: flex-start;    |
| justify-end     | justify-content: flex-end;      |
| justify-center  | justify-content: center;        |
| justify-between | justify-content: space-between; |
| justify-around  | justify-content: space-around;  |
| justify-evenly  | justify-content: space-evenly;  |
| justify-stretch | justify-content: stretch;       |



### Align Items

> Cross axis（垂直方向）



| Class          | Properties               |
| -------------- | ------------------------ |
| items-start    | align-items: flex-start; |
| items-end      | align-items: flex-end;   |
| items-center   | align-items: center;     |
| items-baseline | align-items: baseline;   |
| items-stretch  | align-items: stretch;    |



![img](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071011.png)

### Align Content



| Class            | Properties                    |
| ---------------- | ----------------------------- |
| content-normal   | align-content: normal;        |
| content-center   | align-content: center;        |
| content-start    | align-content: flex-start;    |
| content-end      | align-content: flex-end;      |
| content-between  | align-content: space-between; |
| content-around   | align-content: space-around;  |
| content-evenly   | align-content: space-evenly;  |
| content-baseline | align-content: baseline;      |
| content-stretch  | align-content: stretch;       |



# Grid

## 典型用法



```html
<div class="grid grid-cols-4 gap-6">
  <div class="bg-sky-100">01</div>
  <div class="bg-sky-100">02</div>
  <div class="bg-sky-100">03</div>
  <div class="bg-sky-100">04</div>
</div>
```



## Grid Template Columns

> 参考[官网](https://tailwindcss.com/docs/grid-template-columns)



| Class             | Properties                                         |
| ----------------- | -------------------------------------------------- |
| grid-cols-1       | grid-template-columns: repeat(1, minmax(0, 1fr));  |
| grid-cols-2       | grid-template-columns: repeat(2, minmax(0, 1fr));  |
| grid-cols-3       | grid-template-columns: repeat(3, minmax(0, 1fr));  |
| grid-cols-4       | grid-template-columns: repeat(4, minmax(0, 1fr));  |
| grid-cols-5       | grid-template-columns: repeat(5, minmax(0, 1fr));  |
| grid-cols-6       | grid-template-columns: repeat(6, minmax(0, 1fr));  |
| grid-cols-7       | grid-template-columns: repeat(7, minmax(0, 1fr));  |
| grid-cols-8       | grid-template-columns: repeat(8, minmax(0, 1fr));  |
| grid-cols-9       | grid-template-columns: repeat(9, minmax(0, 1fr));  |
| grid-cols-10      | grid-template-columns: repeat(10, minmax(0, 1fr)); |
| grid-cols-11      | grid-template-columns: repeat(11, minmax(0, 1fr)); |
| grid-cols-12      | grid-template-columns: repeat(12, minmax(0, 1fr)); |
| grid-cols-none    | grid-template-columns: none;                       |
| grid-cols-subgrid | grid-template-columns: subgrid;                    |

## Gap



| Class     | Properties                      |
| --------- | ------------------------------- |
| gap-0     | gap: 0px;                       |
| gap-x-0   | column-gap: 0px;                |
| gap-y-0   | row-gap: 0px;                   |
| gap-px    | gap: 1px;                       |
| gap-x-px  | column-gap: 1px;                |
| gap-y-px  | row-gap: 1px;                   |
| gap-0.5   | gap: 0.125rem; /* 2px */        |
| gap-x-0.5 | column-gap: 0.125rem; /* 2px */ |
| gap-y-0.5 | row-gap: 0.125rem; /* 2px */    |
| gap-1     | gap: 0.25rem; /* 4px */         |
| gap-x-1   | column-gap: 0.25rem; /* 4px */  |
| gap-y-1   | row-gap: 0.25rem; /* 4px */     |





# 大小和边距



## Width



|        | Tailwin                       | CSS                                                          |
| ------ | ----------------------------- | ------------------------------------------------------------ |
|        | w-full                        | width: 100%;                                                 |
|        | w-auto                        | width: auto;                                                 |
|        | w-px                          | width: 1px;                                                  |
| rem    | w-0  w-0.5  w-1  ...  w-96    |                                                              |
| 百分比 | w-1/2  w-1/3  ...  w-11/12    |                                                              |
|        | w-screen  w-svw  w-lvw  w-dvw | width:  100vw;  width:  100svw;  width:  100lvw;  width:  100dvw; |
|        | w-min  w-max  w-fit           | width:  min-content;  width:  max-content;  width:  fit-content; |



### Min-Width



| min-width: 1px;     | min-w-px   |
| ------------------- | ---------- |
| min-width: 100%;    | min-w-full |
|                     | min-w-0    |
| min-width: 0.25rem; | min-w-1    |
|                     |            |
|                     | Min-w-96   |

### Max-Width

同上：`max-w-0`



## Height

同`width`：`h-1`



## Size

| Class   | Properties               |
| ------- | ------------------------ |
| size-0  | width: 0px; height: 0px; |
| size-px | width: 1px; height: 1px; |



## Padding



| 方向         | 格式                 | 例子                                   |
| ------------ | -------------------- | -------------------------------------- |
| single  side | p{t\|r\|b\|l}-{size} |                                        |
| horizontal   | px-{size}            |                                        |
| vertical     | py-{size}            |                                        |
| all  sides   | p-{size}             | p-0  p-px  p-0.5  p-1  ...  p-80  p-96 |

## Margin

同padding，但是多了负的：`-mt-8`

| 方向            | 格式                 | 例子                                   |
| --------------- | -------------------- | -------------------------------------- |
| single  side    | m{t\|r\|b\|l}-{size} |                                        |
| horizontal      | mx-{size}            |                                        |
| vertical        | my-{size}            |                                        |
| all  sides      | m-{size}             | p-0  p-px  p-0.5  p-1  ...  p-80  p-96 |
| negative values |                      | -mt-8                                  |



## Space Between



| 方向              | 格式             | 例子                                   |
| ----------------- | ---------------- | -------------------------------------- |
| horizontal  space | space-x-{amount} |                                        |
| vertical  space   | space-y-{amount} |                                        |
| Reversing         | flex-row-reverse | p-0  p-px  p-0.5  p-1  ...  p-80  p-96 |
| negative values   | -space-x-4       | -mt-8                                  |



# Backround



## Color

[参考](https://tailwindcss.com/docs/background-color)

几个常用颜色

- bg-transparent
- bg-black
- bg-white
- bg-slate-50
- bg-gray-50
- bg-red-50
- bg-sky-50
- bg-indigo-50



数值

- 50
- 100
- 200
- ...
- 900
- 950



透明度

```html
<button class="bg-sky-500/100 ..."></button>
<button class="bg-sky-500/75 ..."></button>
<button class="bg-sky-500/50 ..."></button>
```



# Border



## Width



## Color





# Font

## 字体大小

| Class     | Properties                                                   |
| --------- | ------------------------------------------------------------ |
| text-xs   | font-size: 0.75rem; /* 12px */ line-height: 1rem; /* 16px */ |
| text-sm   | font-size: 0.875rem; /* 14px */ line-height: 1.25rem; /* 20px */ |
|           |                                                              |
| text-base | font-size: 1rem; /* 16px */ line-height: 1.5rem; /* 24px */  |
| text-lg   | font-size: 1.125rem; /* 18px */ line-height: 1.75rem; /* 28px */ |
|           |                                                              |
| text-xl   | font-size: 1.25rem; /* 20px */ line-height: 1.75rem; /* 28px */ |
| text-2xl  | font-size: 1.5rem; /* 24px */ line-height: 2rem; /* 32px */  |
| ...       |                                                              |
| text-9xl  | font-size: 8rem; /* 128px */ line-height: 1;                 |

![image-20240107090708033](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240107090708033.png)

## 字体

| Class      | Properties                                                   |
| ---------- | ------------------------------------------------------------ |
| font-sans  | font-family: ui-sans-serif, system-ui, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji"; |
| font-serif | font-family: ui-serif, Georgia, Cambria, "Times New Roman", Times, serif; |
| font-mono  | font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace; |

![image-20240107090900024](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240107090900024.png)



### 添加自定义字体

[参考](https://tailwindui.com/documentation)



# Hover, focus, and other states



# 响应式





