---
title: "React Tutorial"
description: 
date: 2023-12-02T13:32:18+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# React的本质

- React只是一个UI前端库
- 用`Javascript`来写`HTML/CSS`代码
  - 语法刚好与HTML/CSS类似。



## 与Vue/Angular的区别

- React只是一个Javascript库
  - 它没有路由系统、HTTP请求工具、国际化、表格验证、动画等
- 但，Vue/Angular是一个框架：集成很多工具，一个解决方案。

![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231216143918.png)
![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231216143926.png)



## 思路：Component







> 将页面拆分成Component



![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231216141905.png)



## 历史



| 版本 | 时间 | 主要特性                                                     |
| ---- | ---- | ------------------------------------------------------------ |
| 0.3  | 2011 | Facebook  内部首次发布的 React 版本                          |
| 0.5  | 2011 | 首个公开发布在  GitHub 上的 React 版本                       |
| 0.8  | 2011 | 引入虚拟  DOM (Virtual DOM)，显著提升性能                    |
| 0.9  | 2011 | 引入  JSX，允许在 JavaScript 中编写类似 HTML 的代码          |
| 0.10 | 2013 | 作为开源项目首次发布的  React 版本                           |
| 0.11 | 2013 | 首次在  npm 包管理器上发布                                   |
| 0.12 | 2014 | 支持服务器端渲染的首个版本                                   |
| 0.13 | 2014 | 引入对  ES6 类语法定义组件的支持                             |
| 15   | 2016 | 引入新的基于纤程  (fiber) 的架构，提升性能并增加新特性，如暂停、中止或重用工作 |
| 16   | 2017 | 引入了流式渲染（streaming rendering）  支持异步渲染和错误边界，增强应用的弹性和容错能力 |
| 17   | 2021 | 改进 JSX  转换器，允许不使用 JSX 使用 React                  |
| 18   | 2022 | 服务器组件（Server Components）  并发渲染（Concurrent  Rendering）  自动批处理（Automatic  Batching）  新的Suspense SSR架构（New  Suspense SSR Architecture）  新的Root API  Transition API  Suspense的流式服务器端渲染（Streaming  SSR） |



> 最新版本
>
> [18.2.0](https://github.com/facebook/react/releases)





# 创建第一个React项目

两种方法，我们使用Vite

- Create React APP (CRA)。这是React官方团队出品。
- Vite：速度更快，生成的包也更小





## Vite

> [官网](https://vitejs.dev/guide/)
>
> 最新版本：[5.0](https://github.com/vitejs/vite/releases)



历史版本

| Year | Version | Event                                                        |
| ---- | ------- | ------------------------------------------------------------ |
| 2020 |         | ⭐ Vite, a new front-end build tool, is created by Evan You.  |
| 2021 | 2.0     | ⭐ Release of Vite 2.0, introducing a more flexible plugin system and improved configuration. |
| 2022 | 3.0     | ⭐ Launch of Vite 3.0, featuring enhanced performance and new features. |

> 还是比较新，但是，Github Star已经有50K了。



## 创建项目

```bash
# 创建项目
npm create vite
# 选择React项目

cd react-app
npm install
npm run dev
```



# JSX语法



用Javascript来写HTML，以下是一个组件的JSX代码：

```javascript
function Message(){
    return <h1>Hello World</h1>
}

export default Message;
```

它转成javascript代码（[转换工具](https://babeljs.io/repl)）：

![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231216144829.png)



# 工作原理



更新DOM不是由React完成的，而是由它的同伴React DOM完成的。

查看package.json，发现目前依赖的就是两个库：React和React DOM。

```json
  ...
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  ...
```



index.html指向src/main.tsx

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite + React + TS</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>
```



查看main.tsx：ReactDOM负责渲染我们整个组件树。

根组件 App 包含在另一个叫React.StrictMode的组件里，这个组件不用于显示，而是用于发现一些问题。

```react
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.tsx'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
```



在手机App中，渲染画面使用的是另一个库：ReactNative。

React没有跟具体平台绑定，它对平台是透明的，React创建的应用可以在Web、手机或桌面上使用。



# 创建第一个React组件



有两种方法创建组件： 

- 一种用Javascript类
- 另一种则是Javascript函数：这种更流行

```react
function Message(){
    return <h1>Hello World</h1>
}

export default Message;
```



> 在VSCode中，键入`rfce` 可以快速生成React Component的模板代码。



## 打包多个分段



方法一：Fragment包起来

```react
function ListGroup(){
    return (
        <Fragment>
            <h1>List</h1>
            <ul className="list-group">
                ...
            </ul>
        </Fragment>
  )
}
```

方法二：空内容的尖括号，React会自动使用Fragment包起来。

```react
export const Message = () => {
    return (
        <>
            <h1>title</h1>
            <div>Message</div>
        </>
    )
}

export default Message;
```



最简洁的做法：

```react
export const Message = () =>
        <div>Message</div>


export default Message;
```



```react
export const Message = () =>
    <>
        <h1>title</h1>
        <div>Message</div>
    </>

export default Message;
```









# 参考

https://mp.weixin.qq.com/s/ziTF9AZLOWQGwkxC8RGqxg

