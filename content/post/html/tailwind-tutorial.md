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

待补充



# Hover, focus, and other states



# 响应式





