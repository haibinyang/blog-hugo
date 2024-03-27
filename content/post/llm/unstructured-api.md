---
title: "ETL工具：Unstructured API"
description: 
date: 2024-01-18T11:33:19+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# 介绍

> 本质：一个高级的ETL工具

![image-20240301194257296](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240301194257296.png)



[官网](https://unstructured.io/)

[SaaS版本](https://unstructured-io.github.io/unstructured/apis/saas_api.html#)

[Github](https://github.com/Unstructured-IO/unstructured-api)

[官方文档](https://unstructured-io.github.io/unstructured/apis/usage_methods.html)





# 本地安装

**Docker安装**

```bash
docker run -p 8000:8000 -d --rm --name unstructured-api quay.io/unstructured-io/unstructured-api:latest --port 8000 --host 0.0.0.0
```

**验证**

准备文件

```
sample-docs
├── sample.md
```

测试

```bash
curl -X 'POST' \
  'http://localhost:8000/general/v0/general' \
  -H 'accept: application/json' \
  -H 'Content-Type: multipart/form-data' \
  -F 'files=@sample-docs/sample.md' \
  | jq -C . | less -R
```



[参考文档](https://unstructured-io.github.io/unstructured/apis/usage_methods.html)



# SaaS

```bash
 curl -X 'POST' \
  'https://api.unstructured.io/general/v0/general' \
  -H 'accept: application/json' \
  -H 'Content-Type: multipart/form-data' \
  -H 'unstructured-api-key: <YOUR API KEY>' \
  -F 'files=@sample-docs/family-day.eml' \
  | jq -C . | less -R
```



# 在LangChain中使用

> [LangChain文档](https://js.langchain.com/docs/integrations/document_loaders/file_loaders/unstructured)



## 本地

单个文件

```js
import { UnstructuredLoader } from "langchain/document_loaders/fs/unstructured";

async function main() {
    const options = {
        // 这个很重要
        apiUrl: "http://localhost:8000/general/v0/general",
    };

    const loader = new UnstructuredLoader(
        "sample-docs/sample.md", // 使用相对路径
        options
    );
    const docs = await loader.load();
    console.log(docs);
}

main();
```



整个文件夹

```js
import { UnstructuredDirectoryLoader } from "langchain/document_loaders/fs/unstructured";

async function main() {
    const options = {
        apiUrl: "http://localhost:8000/general/v0/general",
    };

    const loader = new UnstructuredDirectoryLoader(
        "sample-docs",
        options
    );
    const docs = await loader.load();
    console.log(docs);
}

main();
```



响应

```json
[
  Document {
    pageContent: '自我介绍',
    metadata: {
      page_number: 1,
      languages: [Array],
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'Title'
    }
  },
  Document {
    pageContent: '基本信息',
    metadata: {
      page_number: 1,
      languages: [Array],
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'Title'
    }
  },
  Document {
    pageContent: '大家好，我是张三，一名软件工程师，专注于人工智能和机器学习领域的研究与开发。',
    metadata: {
      page_number: 1,
      languages: [Array],
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'Title'
    }
  },
  Document {
    pageContent: '教育背景',
    metadata: {
      page_number: 1,
      languages: [Array],
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'Title'
    }
  },
  Document {
    pageContent: '我在北京大学计算机科学与技术系获得了本科学位，并在清华大学获得了人工智能方向的硕士学位。',
    metadata: {
      page_number: 1,
      languages: [Array],
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'Title'
    }
  },
  Document {
    pageContent: '工作经验',
    metadata: {
      page_number: 1,
      languages: [Array],
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'Title'
    }
  },
  Document {
    pageContent: '2018-2020：在字节跳动担任软件工程师，参与了多个人工智能项目的研发工作。',
    metadata: {
      page_number: 1,
      languages: [Array],
      parent_id: '0fa18c56f808827ae9f771d61a971b86',
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'ListItem'
    }
  },
  Document {
    pageContent: '2020至今：在OpenAI工作，负责机器学习模型的研究与开发。',
    metadata: {
      page_number: 1,
      languages: [Array],
      parent_id: '0fa18c56f808827ae9f771d61a971b86',
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'ListItem'
    }
  },
  Document {
    pageContent: '技能',
    metadata: {
      page_number: 1,
      languages: [Array],
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'Title'
    }
  },
  Document {
    pageContent: '精通Python和C++编程语言。',
    metadata: {
      page_number: 1,
      languages: [Array],
      parent_id: '99aea2f9131ad6dad0e5f8eeea074688',
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'ListItem'
    }
  },
  Document {
    pageContent: '深入理解深度学习框架，如TensorFlow和PyTorch。',
    metadata: {
      page_number: 1,
      languages: [Array],
      parent_id: '99aea2f9131ad6dad0e5f8eeea074688',
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'ListItem'
    }
  },
  Document {
    pageContent: '在图像识别领域有丰富的实践经验。',
    metadata: {
      page_number: 1,
      languages: [Array],
      parent_id: '99aea2f9131ad6dad0e5f8eeea074688',
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'ListItem'
    }
  },
  Document {
    pageContent: '熟悉自然语言处理技术。',
    metadata: {
      page_number: 1,
      languages: [Array],
      parent_id: '99aea2f9131ad6dad0e5f8eeea074688',
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'ListItem'
    }
  },
  Document {
    pageContent: '联系方式',
    metadata: {
      page_number: 1,
      languages: [Array],
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'Title'
    }
  },
  Document {
    pageContent: '邮箱：zhangsan@example.com',
    metadata: {
      page_number: 1,
      languages: [Array],
      parent_id: '323a881ebd474ae0373e9226963df8d2',
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'ListItem'
    }
  },
  Document {
    pageContent: 'GitHub：https://github.com/zhangsan',
    metadata: {
      page_number: 1,
      languages: [Array],
      parent_id: '323a881ebd474ae0373e9226963df8d2',
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'ListItem'
    }
  },
  Document {
    pageContent: '个人格言',
    metadata: {
      page_number: 1,
      languages: [Array],
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'Title'
    }
  },
  Document {
    pageContent: '勇于探索，不断学习，用技术改变世界。',
    metadata: {
      page_number: 1,
      languages: [Array],
      filename: 'sample.md',
      filetype: 'text/markdown',
      category: 'Title'
    }
  }
]
```





### 对比LangChain的Splitter

```js
import { RecursiveCharacterTextSplitter } from "langchain/text_splitter";

async function main() {
    const text = `
    # 自我介绍

    ## 基本信息
    
    大家好，我是张三，一名软件工程师，专注于人工智能和机器学习领域的研究与开发。
    
    ### 教育背景
    
    我在北京大学计算机科学与技术系获得了本科学位，并在清华大学获得了人工智能方向的硕士学位。
    
    #### 工作经验
    
    - **2018-2020**：在字节跳动担任软件工程师，参与了多个人工智能项目的研发工作。
    - **2020至今**：在OpenAI工作，负责机器学习模型的研究与开发。
    
    ##### 技能
    
    1. 精通Python和C++编程语言。
    2. 深入理解深度学习框架，如TensorFlow和PyTorch。
       1. 在图像识别领域有丰富的实践经验。
       2. 熟悉自然语言处理技术。
    
    ###### 联系方式
    
    - 邮箱：[zhangsan@example.com](mailto:zhangsan@example.com)
    - GitHub：[https://github.com/zhangsan](https://github.com/zhangsan)
    
    ###### 个人格言
    
    > 勇于探索，不断学习，用技术改变世界。    
`;

    const splitter = RecursiveCharacterTextSplitter.fromLanguage("markdown", {
        chunkSize: 500,
        chunkOverlap: 0,
    });
    const output = await splitter.createDocuments([text]);

    console.log(output);
}

main();
```



**结果**

```json
[
  Document { pageContent: '# 自我介绍', metadata: { loc: [Object] } },
  Document {
    pageContent: '## 基本信息\n' +
      '    \n' +
      '    大家好，我是张三，一名软件工程师，专注于人工智能和机器学习领域的研究与开发。\n' +
      '    \n' +
      '    ### 教育背景\n' +
      '    \n' +
      '    我在北京大学计算机科学与技术系获得了本科学位，并在清华大学获得了人工智能方向的硕士学位。\n' +
      '    \n' +
      '    #### 工作经验\n' +
      '    \n' +
      '    - **2018-2020**：在字节跳动担任软件工程师，参与了多个人工智能项目的研发工作。\n' +
      '    - **2020至今**：在OpenAI工作，负责机器学习模型的研究与开发。\n' +
      '    \n' +
      '    ##### 技能\n' +
      '    \n' +
      '    1. 精通Python和C++编程语言。\n' +
      '    2. 深入理解深度学习框架，如TensorFlow和PyTorch。\n' +
      '       1. 在图像识别领域有丰富的实践经验。\n' +
      '       2. 熟悉自然语言处理技术。\n' +
      '    \n' +
      '    ###### 联系方式\n' +
      '    \n' +
      '    - 邮箱：[zhangsan@example.com](mailto:zhangsan@example.com)',
    metadata: { loc: [Object] }
  },
  Document {
    pageContent: '- GitHub：[https://github.com/zhangsan](https://github.com/zhangsan)\n' +
      '    \n' +
      '    ###### 个人格言\n' +
      '    \n' +
      '    > 勇于探索，不断学习，用技术改变世界。',
    metadata: { loc: [Object] }
  }
]
```

## SaaS

```js
import { UnstructuredLoader } from "langchain/document_loaders/fs/unstructured";

async function main() {
    const options = {
        apiKey: "MY_API_KEY",
        apiUrl: "MY_API_KEY",
    };

    const loader = new UnstructuredLoader(
        "./sample.md",
        options
    );
    const docs = await loader.load();
    console.log(docs);
}

main();
```

