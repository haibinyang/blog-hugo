---
title: "Geist字体"
description: 
date: 2024-01-06T13:32:59+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---

# 字体



| 字体风格                        | 特点                 | 适用场景             | 示例                     |
| ------------------------------- | -------------------- | -------------------- | ------------------------ |
| 无衬线体 (Sans-serif)           | 简洁，没有衬线       | 屏幕显示，小字号     | Arial, Helvetica         |
| 衬线体 (Serif)                  | 有装饰性的衬线       | 书籍，报纸印刷       | Times New Roman, Georgia |
| 等宽字体 (Monospace)            | 每个字符占据相同空间 | 代码编写，计算机终端 | Courier New              |
| 手写体 (Script)                 | 模仿手写笔迹         | 邀请函，贺卡         | -                        |
| 装饰性字体 (Display/Decorative) | 独特设计和装饰       | 标题，特殊场合       | -                        |

![image-20240106114304918](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240106114304918.png)

# Geist

[Geist字体](https://vercel.com/font)是Vercel官方出的字体

有两种:

- Geist Sans：无衬体
- Geist Mono：等宽



# 集成到Next.js

1、安装

```bash
npm install geist
```

2、导入className，在`app/layout.js`:

```tsx
import { GeistSans } from 'geist/font/sans'
import { GeistMono } from 'geist/font/mono'

export default function RootLayout({
  children,
}) {
  return (
    <html lang="en" className={`${GeistSans.variable} ${GeistMono.variable}`}>
      <body>{children}</body>
    </html>
  )
}
```

3、配置`tailwind.config.js`:

```js
module.exports = {
  theme: {
    extend: {
      fontFamily: {
        sans: ['var(--font-geist-sans)'],
        mono: ['var(--font-geist-mono)'],
      },
    },
  },
}
```



[教程](https://www.npmjs.com/package/geist?activeTab=readme#installation)

