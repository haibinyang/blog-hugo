---
layout: post
title:  "Node.js基础"
date:   2020-03-19 07:58:58 +0800
categories: js nodejs
---



## 基础



### package.json 文件

```json
{
  "name": "n1",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "aws-iot-device-sdk": "^2.2.3"
  }
}

```



使用 `npm install node_module –save` 自动更新 dependencies 字段值。