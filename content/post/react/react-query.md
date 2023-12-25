---
title: "React Query"
description: 
date: 2023-12-25T17:40:43+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# 动机

在React中执行HTTP请求

- 使用useEffect
- 自己处理：错误、重试和缓存等

![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231225174332.png)



使用React Query来完成这些工作：

![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231225174203.png)





# 集成

**安装库**

```
npm i @tanstack/react-query
```



**使用Provider将组件包起来**

- 创建一个queryClient对象
- 将App组件用 QueryClientProvider包起来
  - QueryClientProvider的client属性就是刚建的queryClient

```react
// main.tsx

......
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
......

const queryClient = new QueryClient();

ReactDOM.createRoot(document.getElementById("root") as HTMLElement).render(
  <React.StrictMode>
    <QueryClientProvider client={queryClient}>
      <App />
    </QueryClientProvider>
  </React.StrictMode>
);
```



**获取数据**

导入useQuery

useQuery参数是一个对象，有两个必备属性：

- queryKey：查询的键值，是一个列表，
- queryFn：查询的函数，返回一个Promise

useQuery返回一个对象，该对象有 data、error等属性，这里我解构使用了data并重命为 todos



```react
// TodoList.tsx

import { useQuery } from "@tanstack/react-query";
import axios from "axios";

interface Todo {
  id: number;
  title: string;
  userId: number;
  completed: boolean;
}

const TodoList = () => {
  const fetchTodos = () =>
    axios
      .get<Todo[]>("https://jsonplaceholder.typicode.com/todos")
      .then((res) => res.data);

  const { data: todos } = useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodos,
  });

  return (
    <ul className="list-group">
      {todos?.map((todo) => (
        <li key={todo.id} className="list-group-item">
          {todo.title}
        </li>
      ))}
    </ul>
  );
};

export default TodoList;
```





[参考](https://mp.weixin.qq.com/s?src=11&timestamp=1703497275&ver=4978&signature=d6RuLUCBYxHBd4WhcM5znceUF1hKvE17b0k1rXQqKE1u5Kbl6LT73pGluEe1ba6ysSejJUinDN8PFGeYsaM4qFk9avHWAs2myqJa2M5OFTYqfgkvLm61Tnsb6*StRdUI&new=1)