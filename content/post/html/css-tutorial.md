---
title: "CSS Tutorial"
description: 
date: 2023-12-20T13:21:55+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# 响应式

> 参考[阮一峰](https://www.ruanyifeng.com/blog/2012/05/responsive_web_design.html)



## demo

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Responsive Demo with REM</title>
    <style>
        /* Set the base font size for the document */
        html {
            font-size: 16px; /* This is typically the default size */
        }

        /* Responsive font sizes */
        body {
            font-size: 1rem; /* 16px */
        }

        h1 {
            font-size: 2rem; /* 32px */
        }

        /* Responsive layout using REM */
        .container {
            width: 80%;
            margin: 0 auto;
            padding: 2rem; /* Padding around the container */
        }

       
    </style>
</head>
<body>
    <div class="container">
        <h1>Welcome to the Responsive Demo</h1>
        <p>This is an example of a responsive design using REM units.</p>
    </div>
</body>
</html>

```



```html
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

- viewport是网页默认的宽度和高度，上面这行代码的意思是，网页宽度默认等于屏幕宽度（width=device-width）
- 原始缩放比例（initial-scale=1）为1.0，即网页初始大小占屏幕面积的100%。



## REM



REM中的R表示root

- **默认浏览器字体大小**：大多数浏览器的默认字体大小为16像素。如果您没有在`<html>`元素上明确设置字体大小，那么`1rem`通常等于16像素。
- **在`<html>`元素上自定义字体大小**：如果您在`<html>`元素上设置了自定义字体大小，那么`1rem`将等于该字体大小。例如，如果您在`<html>`元素上设置`font-size: 20px`，那么`1rem`将等于20像素。



```css
html {
    font-size: 20px; /* 这设置1rem等于20像素 */
}
```





# display属性

重点有这些：

- none：不显示
- block
- flex
- grid



## 所有可选值

| 序号 | 属性值       | 描述                                                       |
| ---- | ------------ | ---------------------------------------------------------- |
| 1    | none         | 元素不会被显示。                                           |
| 2    | block        | 元素显示为块级元素，新行开始。                             |
| 3    | inline       | 元素在行内显示，不会开始新行。                             |
| 4    | inline-block | 元素在行内显示，但表现得像块级元素（可以设置宽度和高度）。 |
| 5    | flex         | 元素成为灵活的容器，使用flexbox模型排列子元素。            |
| 6    | grid         | 元素成为网格容器，使用网格布局排列子元素。                 |
| 7    | table        | 元素显示像一个表格。                                       |
| 8    | table-row    | 元素显示像表格中的一行（`<tr>`）。                         |
| 9    | table-cell   | 元素显示像表格中的一个单元格（`<td>` 或 `<th>`）。         |
| 10   | list-item    | 元素显示像列表项，通常带有列表标记。                       |

## 默认值

div默认是block，占据一整行。

Flex

| HTML元素类型                                | 默认的 `display` 属性值 |
| ------------------------------------------- | ----------------------- |
| 块级元素 (如 `<div>`, `<p>`, `<h1>`-`<h6>`) | block                   |
| 行内元素 (如 `<span>`, `<a>`, `<img>`)      | inline                  |





# Flex Box

> 阮一峰的[文章](https://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)非常好。



## 基本概念

采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。

它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"。

容器默认存在两根轴：

- 水平的主轴（main axis）
- 垂直的交叉轴（cross axis）



主轴的开始位置（与边框的交叉点）叫做`main start`，结束位置叫做`main end`；

交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`。



![img](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071004.png)





## 容器的属性

|                 |                                                              |
| --------------- | ------------------------------------------------------------ |
| flex-direction  | row（默认值） \| row-reverse \| column \|  column-reverse;   |
| flex-wrap       | nowrap（默认值） \| wrap \| wrap-reverse                     |
| flex-flow       | `flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。 |
| justify-content | main axis（水平方向）<br/>flex-start（默认值） \| flex-end \| center \| space-between \| space-around; |
| align-items     | cross axis（垂直方向）<br/>flex-start \| flex-end \| center \| baseline \| stretch（默认值）; |
| align-content   | 定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。 |



方向

![img](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071005.png)



nowrap

![img](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071007.png)



wrap

![img](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071008.jpg)



justify-content

![img](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071010.png)

align-items

![img](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015071011.png)

# Grid

> 阮一峰的[文章](https://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)

还没仔细学习。





