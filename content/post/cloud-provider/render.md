---
title: "Render"
description: 
date: 2024-02-03T12:27:03+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# 定价和套餐

[链接](https://render.com/pricing)

1. **价格结构**：
   - **个人计划**：免费
   - **团队计划**：每月19美元
   - **组织计划**：每月29美元
2. **构建和部署**：
   - **免费构建管道分钟数**和**免费带宽**随计划不同而增加：
     - 个人计划提供500分钟/月和100GB带宽；
     - 团队计划提供500分钟/月和500GB带宽；
     - 组织计划提供500分钟/月和1TB带宽。
3. **数据存储**：
   - 所有计划都提供托管的PostgreSQL和Redis服务，但是高级功能如自动备份、读副本、点对点恢复可能在不同计划中有所差异。
4. **便利性和监控**：
   - **日志保留**时间从个人计划的7天，到团队计划的14天，组织计划的30天。
   - 构建保留数量和自动缩放功能也根据计划不同而有所差异。



计算：

存储：1GB=2元人民币/月





# 支持的Python的版本



**默认支持的版本**

3.11

| Service Creation Date       | Default Python Version |
| :-------------------------- | :--------------------- |
| `2024-01-02` and later      | `3.11.7`               |
| `2023-12-04` - `2024-01-01` | `3.11.6`               |
| Before `2023-11-01`         | `3.7.10`               |



**可以指定版本**

在环境变量中指定版本。

Specify a different Python version by setting your service’s `PYTHON_VERSION` [environment variable](https://docs.render.com/configure-environment-variables) to a valid Python version (e.g., `3.9.13`). You can specify any released version from `3.7.0` onward.



# blueprint: render.yaml

[blueprint-spec](https://docs.render.com/blueprint-spec#type)



**Type**

- `web` for a web service
- `worker` for a background worker
- `pserv` for a private service
- `cron` for a cron job





# 部署FastAPI

https://docs.render.com/deploy-fastapi





开源的项目

dokku

https://dokku.com/



caprover

https://github.com/caprover/caprover