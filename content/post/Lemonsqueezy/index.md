---
title: "Lemonsqueezy教程：Stripe的最好替代品"
description: 
date: 2023-11-30T11:07:30+08:00
image: cover.png
math: 
license: 
hidden: false
comments: true
draft: false

---


# Lemonsqueezy优点

- 申请门槛低：国内用户也可以申请，不需要公司资质

- 支付微信支付和支付宝：详情见[这里](https://docs.lemonsqueezy.com/help/checkout/payment-methods)

- 支持试用

- 有佣金系统

- 有折扣码系统

  

# 对比支付网关

| 服务名称     | 费率            | 支持的支付方式                         | 支持的国家和货币                   | 安全性           | 客户服务和支持         |
| ------------ | --------------- | -------------------------------------- | ---------------------------------- | ---------------- | ---------------------- |
| PayPay       | 2.9% + 0.30美元 | 信用卡、借记卡、银行转账、PayPal余额等 | 覆盖多国和货币，支持多种语言       | 金融交易保护     | 可能提供24/7支持       |
| Stripe       | 2.9% + 30美分   | 信用卡、借记卡、银行转账、PayPal余额等 | 覆盖多国和货币，支持多种语言       | 提供保障         | 可能提供24/7支持       |
| Paddle       | 1.5% + 20p      | 只支持信用卡和PayPal付款               | 覆盖范围较小，但支持多种货币和语言 | 提供保障         | 可能只在工作日提供帮助 |
| LemonSqueezy | 5%-7%           | 只接受信用卡付款                       | 覆盖范围较小，但支持多种货币和语言 | 缺乏顶级安全标准 | 可能只在工作日提供帮助 |



LemonSqueezy的费率还是比较高，可靠性也不如Stripe，但从后台管理系统来看，很专业的团队。



# 使用教程



### 申请帐号

点击[这里](https://lemonsqueezy.io.)申请



### 创建Store

商店里面可以建立多款产品(Products)、每个产品的购买可以生成对应的License，后续包括产品订单(Orders)、订阅(Subscriptions)、客户(Customers)、折扣(Discounts)。这里面最重要的是产品的建立(Products).



#### StoreId

查询[这里](https://app.lemonsqueezy.com/settings/stores)查看所有Store，每个Store后面有一个#开头的数字。

![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231129175647.png)

#### 激活商店

会要求你填写商店的用途，需要你认真填写。

#### 货币

 Lemonsqueezy 仅支持每家商店一种货币。



### 创建Product

> SaaS产品举例

|          | 月                | 年                                                           |
| -------- | ----------------- | ------------------------------------------------------------ |
| Free     | 不用创建          |                                                              |
| Standare | 创建第一个Product | 在第一个Product的基础上创建2个`variants`：按月** **和 按年** |
| Pro      | 创建第二个Product | 同上                                                         |



两个Product

<img src="https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231129174003.png" style="zoom: 67%;" />



<img src="https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231129174139.png" style="zoom:67%;" />

#### 预览支付界面



<img src="https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231129174534.png" style="zoom:67%;" />

![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231129175244.png)

#### 免费试用

若要提供免费试用，可以将“试用期”设置为要提供试用的天数。

![](https://docs.supastarter.dev/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Flemonsqueezy-trial.d4b8085d.png&w=3840&q=75)



### Test Mode

打开左下角的`test mode`按钮，你可以自己测试。

<img src="https://docs.lemonsqueezy.com/_next/image?url=%2Fimages%2FDocs-02-Test-Mode.png&w=1920&q=75" alt="img" style="zoom: 25%;" />



#### 测试数据

可以使用以下的模拟数据来测试

- Visa: `4242 4242 4242 4242`
  - 有效日期（例如 12/25）
  - 任何三位数的 CVC（例如 123）
- Insufficient funds: `4000 0000 0000 9995`
- Expired card: `4000 0000 0000 0069`

更多见[官方文档](https://docs.lemonsqueezy.com/help/getting-started/test-mode)



#### 测试完成后可以复制到生产环境

<img src="https://docs.lemonsqueezy.com/_next/image?url=%2Fimages%2FDocs-03-Test-Mode-Copy-to-Live.png&w=1920&q=75" alt="img" style="zoom: 25%;" />



### API接入

#### 创建 API 密钥

点击[这里](https://app.lemonsqueezy.com/settings/api)创建API密钥

在设置中，然后单击加号按钮。您需要为您的 API 密钥命名，然后单击“创建”按钮。创建 API 密钥后，您需要复制 API 密钥，以便在运行 CLI 时在集成设置中使用它。

##### Test mode

在Test mode创建的API Key可以获取Test mode下的数据，这样可以方便开发和测试。



#### 使用curl测试API Key

你开发的系统，通过RESTful API与LemonSqueezy交互，具体见[API说明文档](https://docs.lemonsqueezy.com/api/products#list-all-products)

> 例如，可以查询所有的产品

```http
curl "https://api.lemonsqueezy.com/v1/products" 
     -H 'Accept: application/vnd.api+json' 
     -H 'Content-Type: application/vnd.api+json' 
     -H 'Authorization: Bearer {api_key}'
```



#### 回调(Webhook)

若要将当前订阅状态和其他信息同步到数据库，需要设置 Webhook。

- 点击[这里](https://app.lemonsqueezy.com/settings/webhooks)创建Webhook，点击右上角的`+`号

- 您必须输入签名密钥，您可以通过在终端中运行以下命令来获取该密钥：

```bash
  openssl rand -base64 40
```

- 复制生成的字符串并将其粘贴到 Signing secret 字段中。

- 然后选择所有事件

- 回调函数：根据你的系统来填写

- 单击“保存 webhook”按钮。

<img src="https://docs.supastarter.dev/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Flemonsqueezy-webhook.ec4bd4a5.png&w=3840&q=75" alt="Create a webhook" style="zoom: 50%;" />



#### 本地开发

##### 使用ngrok中转

基本原理：使用ngrok中转Lemonsqueezy的callback回调

安装Ngrok：具体见[官网文档](https://ngrok.com/docs/getting-started/)

启用Ngrok

```bash
ngrok http 3000
```

![ngrok](https://docs.supastarter.dev/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fngrok.d63fbbd2.png&w=3840&q=75)

分配一个转发的URL给你。

在test mode下，修改Webhook的URL中的主机地址为Ngrok提供给你的URL。



##### 在Node.js项目中集成Ngrok工具

将以下命令添加到 `package.json` 文件中：

```bash
"ngrok": "npx ngrok http 3000"
您可以使用以下命令运行它：
```

```bash
npm run ngrok
```



## 参考

https://juejin.cn/post/7218916901593415736

[Makerkit](https://makerkit.dev/docs/next-supabase/lemon-squeezy#step-4-configure-lemon-squeezy-plans-in-the-makerkit-configuration)

[Supastarter](https://docs.supastarter.dev/2.0/nextjs/billing/lemonsqueezy#create-webhook)



做得比较好的产品的付费设计

https://podwise.xyz/dashboard/billing





