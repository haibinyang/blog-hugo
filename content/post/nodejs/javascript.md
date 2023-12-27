---
title: "Javascript"
description: 
date: 2023-12-27T15:15:29+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# 接口



## 继承

`extends` 关键字用于接口 (interface) 的继承。这使得一个接口可以继承另一个接口的成员，从而实现接口的扩展和重用。

例如：

```typescript
interface Person {
    name: string;
    age: number;
}

interface Employee extends Person {
    employeeId: number;
}

let employee: Employee = {
    name: "张三",
    age: 30,
    employeeId: 12345
};
```

