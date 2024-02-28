---
title: "ElasticSearch入门"
description: 
date: 2024-02-04T13:34:19+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# 链接

[官网](https://www.elastic.co/elasticsearch)

[Github](https://github.com/elastic/elasticsearch)

[Docker镜像官网](https://hub.docker.com/_/elasticsearch)

[官网关于向量数据库的介绍](https://www.elastic.co/cn/what-is/vector-search)



# 历史和版本

| 年份 | 版本 | 事件                                                         |
| ---- | ---- | ------------------------------------------------------------ |
| 2010 | 0.4  | ⭐Elasticsearch首次发布                                       |
| 2012 | 0.20 | ⭐添加了对分布式搜索的支持                                    |
| 2014 | 1.0  | ⭐发布了正式的1.0版本，包含了更多的特性，如快照和恢复、分片分配等 |
| 2015 | 2.0  | ⭐性能改进和安全性增强                                        |
| 2017 | 5.0  | ⭐引入了全新的索引格式，提高了搜索速度和索引压缩              |
| 2018 | 6.0  | 改进了多个领域的性能和稳定性                                 |
| 2020 | 7.0  | ⭐引入了新的集群协调机制，提升了稳定性和可扩展性              |
| 2022 | 8.0  | ⭐推出了Elasticsearch 8.0，包含了诸多新特性和性能提升         |



**最近的版本**

- 7.17.14
- 7.17.15
- 7.17.16
- 7.17.17
- 8.11.0
- 8.11.1
- 8.11.2
- 8.11.3
- 8.11.4
- 8.12.0：最新版本（截止到2024年2月）



**什么时候开始支持向量数据库**

Elasticsearch开始支持向量存储的功能是在7.0版本中，通过引入`dense_vector`字段类型。

从8.0版本开始，Elasticsearch通过技术预览的`_knn_search` API端点，支持向量搜索。



# 安装

> [非常好的教程](https://mp.weixin.qq.com/s/3FGdEcyUQJeGr4y_PcZPJg)



**使用Docker安装**

> [官方教程](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)



拉取镜像

```bash
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.12.0
```

创建实例

```bash
docker run -d --name es -p 9200:9200 -e "discovery.type=single-node" -m 1GB docker.elastic.co/elasticsearch/elasticsearch:8.12.0
```

修改密码

```bash
# 进入 ES 容器
$ docker exec -it es bash

$ elasticsearch@6b8c334abc11:~$ pwd
/usr/share/elasticsearch

$ elasticsearch@6b8c334abc11:~$ ls
LICENSE.txt  NOTICE.txt  README.asciidoc  bin  config  data  jdk  lib  logs  modules  plugins

# 重置密码
$ bin/elasticsearch-reset-password -u elastic -i
```



确认是否安装成功

访问https://localhost:9200/

帐号：elastic

密码：刚刚设置的

响应

```json
{
  name: "6b8c334abc11",
  cluster_name: "docker-cluster",
  cluster_uuid: "q59Kp9WpR9yZNTBsD3v4IQ",
  version: {
    number: "8.12.0",
    build_flavor: "default",
    build_type: "docker",
    build_hash: "1665f706fd9354802c02146c1e6b5c0fbcddfbc9",
    build_date: "2024-01-11T10:05:27.953830042Z",
    build_snapshot: false,
    lucene_version: "9.9.1",
    minimum_wire_compatibility_version: "7.17.0",
    minimum_index_compatibility_version: "7.0.0"
  },
  tagline: "You Know, for Search"
}
```



关闭SSL

```bash
cd ~/Documents/tools/elastic #随便找个文件

# 拷贝配置文件
docker cp es:/usr/share/elasticsearch/config ./config
# 关闭 ES 容器
docker rm -f es
```

`config`文件夹包含了`elascitsearch.yml`和其他配置文件，然后我们修改`elascitsearch.yml`文件来关闭 SSL 认证，修改内容如下：

```bash
# Enable encryption for HTTP API client connections, such as Kibana, Logstash, and Agents
xpack.security.http.ssl:
-  enabled: true
+  enabled: false
  keystore.path: certs/http.p12

# Enable encryption and mutual authentication between cluster nodes
xpack.security.transport.ssl:
-  enabled: true
+  enabled: false
  verification_mode: certificate
  keystore.path: certs/transport.p12
  truststore.path: certs/transport.p12
```



创建第二个es

```bash
cd ~/Documents/tools/elastic #随便找个文件

docker run -d --name es -p 9200:9200 -v "$PWD/config":/usr/share/elasticsearch/config -e "discovery.type=single-node" -m 1GB docker.elastic.co/elasticsearch/elasticsearch:8.12.0
```

> 需要重置密码





# ES 监控工具

想要查看 ES 中的数据，如果是使用命令行工具的话可能不太方便，因此我们需要一个 GUI 工具，这里推荐**elasticvue**[2]，一个基于浏览器的 ES GUI 工具，安装也非常简单，同样是使用 docker 来进行安装：

```
docker run -p 9080:8080 --name elasticvue -d cars10/elasticvue
```

然后我们在浏览器中输入`http://localhost:9080`来访问 elasticvue，进到首页后点击`ADD ELASTICSEARCH CLUSTER`按钮。

根据上图上半部分的`Configure`提示，需要修改 ES 的配置文件`elascitsearch.yml`以接入 elasticvue，修改内容可以参考图中的`Configure`部分，

```bash
# 允许 CORS 请求来自 http://localhost:9080
http.cors.enabled: true
http.cors.allow-origin: "http://localhost:9080"

# 如果你的集群使用授权:
http.cors.allow-headers: X-Requested-With,Content-Type,Content-Length,Authorization
```

修改完后重启 ES 容器即可。

```
docker restart es
```

然后在 elasticvue 中添加 ES 集群，输入 ES 的地址`http://localhost:9200`，选择`Basic auth`输入用户名和密码，这样就可以连上我们的 ES 服务了。



![image-20240204232602805](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/image-20240204232602805.png)



# 在LlamaIndex中使用ES

[非常好的教程](https://mp.weixin.qq.com/s/3FGdEcyUQJeGr4y_PcZPJg)

[LlamaIndex官方](https://docs.llamaindex.ai/en/stable/examples/vector_stores/ElasticsearchIndexDemo.html)





# 算法和策略



ANN

BM25F



### 人工神经网络 (ANN) 算法

传统的最近邻算法（如 k 最近邻算法 (kNN)）会导致执行时间过长并占用计算资源。ANN 牺牲了完美准确性，以换取在高维度嵌入空间中实现大规模高效运行。



### 基于元数据进行筛选

使用元数据筛选矢量搜索结果。通过应用与近似最近邻 (ANN) 搜索一致的筛选条件，在不牺牲速度的情况下保持查全率。



### 重新排序搜索结果

矢量相似度可以解释为相似度分数，您可以结合其他数据对该分数重新排序。这包括矢量搜索数据库中已有的静态字段，以及应用 Machine Learning 模型获得的新属性。



### 混合评分

为了进一步优化，您可以将矢量相似度与 BM25F 评分相结合，这称为混合评分。使用混合评分，可让您在实现 BM25F 的同时按矢量相似度对图像进行排序，从而提供更好的文本排名。
