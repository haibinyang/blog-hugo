---
title: "OpenAI API参考"
description: 
date: 2024-02-29T11:33:19+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: true
---



# 测试



```bash
curl https://api.openai.com/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [
      {
        "role": "system",
        "content": "You are a helpful assistant."
      },
      {
        "role": "user",
        "content": "Hello!"
      }
    ]
  }'
```



# 代理接口

使用CloudFlare

[官方教程](https://developers.cloudflare.com/ai-gateway/get-started/connecting-applications/)



将Base URL

```
https://api.openai.com/v1
```

替换成

```
https://gateway.ai.cloudflare.com/v1/[xxx]/[yyy]/openai
```



例如

```bash
curl https://gateway.ai.cloudflare.com/v1/45a93dd9034ce3d27b263a074d05f09a/ververv/openai/chat/completions \
```





# Chat接口定义

> [官网](https://platform.openai.com/docs/api-reference/chat/create)





# 设置响应的格式

> [官网](https://platform.openai.com/docs/api-reference/chat/create#chat-create-response_format)

response_format

Must be one of `text` or `json_object`.

Setting to `{ "type": "json_object" }` enables JSON mode, which guarantees the message the model generates is valid JSON.



**Important:** when using JSON mode, you **must** also instruct the model to produce JSON yourself via a system or user message. Without this, the model may generate an unending stream of whitespace until the generation reaches the token limit, resulting in a long-running and seemingly "stuck" request. Also note that the message content may be partially cut off if `finish_reason="length"`, which indicates the generation exceeded `max_tokens` or the conversation exceeded the max context length.



# Function Call/Tools

> [官网](https://platform.openai.com/docs/api-reference/chat/create#chat-create-tools)



Tools：A list of tools

每个工具是一个对象，每个对象有

type：Currently, only `function` is supported.

function：name，description，parameters

每个parameters的定义：

名字，参数类型，描述，可选值，哪些参数是required

## 范例

**请求**

```bash
curl https://api.openai.com/v1/chat/completions \
-H "Content-Type: application/json" \
-H "Authorization: Bearer $OPENAI_API_KEY" \
-d '{
  "model": "gpt-3.5-turbo",
  "messages": [
    {
      "role": "user",
      "content": "What is the weather like in Boston?"
    }
  ],
  "tools": [
    {
      "type": "function",
      "function": {
        "name": "get_current_weather",
        "description": "Get the current weather in a given location",
        "parameters": {
          "type": "object",
          "properties": {
            "location": {
              "type": "string",
              "description": "The city and state, e.g. San Francisco, CA"
            },
            "unit": {
              "type": "string",
              "enum": ["celsius", "fahrenheit"]
            }
          },
          "required": ["location"]
        }
      }
    }
  ],
  "tool_choice": "auto"
}'

```

**响应**

```json
{
  "id": "chatcmpl-abc123",
  "object": "chat.completion",
  "created": 1699896916,
  "model": "gpt-3.5-turbo-0125",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": null,
        "tool_calls": [
          {
            "id": "call_abc123",
            "type": "function",
            "function": {
              "name": "get_current_weather",
              "arguments": "{\n\"location\": \"Boston, MA\"\n}"
            }
          }
        ]
      },
      "logprobs": null,
      "finish_reason": "tool_calls"
    }
  ],
  "usage": {
    "prompt_tokens": 82,
    "completion_tokens": 17,
    "total_tokens": 99
  }
}
```



## 大的框架



```json
{
  "model": "gpt-3.5-turbo",
  "messages": [
    {
      "role": "user",
      "content": "What is the weather like in Boston?"
    }
  ],
  "tools": [ // tool列表，与model同级
    { // 具体某个tool
      "type": "function", // 类型：暂时只支持function
      "function": {
        "name": "get_current_weather",
        "description": "Get the current weather in a given location",
        "parameters": {
          // 更具体定义
        }
      }
    }
  ],
  "tool_choice": "auto" // 选择哪个tool，与model同级
}
```

### parameters

```json
{
  "type": "object",
  "properties": {
    "location": {
      "type": "string",
      "description": "The city and state, e.g. San Francisco, CA"
    },
    "unit": {
      "type": "string",
      "enum": [
        "celsius",
        "fahrenheit"
      ]
    }
  },
  "required": ["location"],
}
```

