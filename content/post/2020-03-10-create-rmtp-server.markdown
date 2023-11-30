---
layout: post
title:  "搭建直播服务器"
date:   2020-03-10 07:58:58 +0800
categories: rmtp ffmpeg nginx
---



## 公开的流



SRS测试流迁移到了[r.ossrs.net](http://r.ossrs.net/)，包括：

- [rtmp://r.ossrs.net/live/livestream](http://ossrs.net/players/srs_player.html?app=live&stream=livestream&server=r.ossrs.net&port=1935&autostart=true&vhost=r.ossrs.net)
- [http://r.ossrs.net:8080/live/livestream.flv](http://ossrs.net/players/srs_player.html?app=live&stream=livestream.flv&server=r.ossrs.net&port=8080&autostart=true&vhost=r.ossrs.net&schema=http)
- [http://r.ossrs.net/live/livestream.m3u8](http://ossrs.net/players/srs_player.html?app=live&stream=livestream.m3u8&server=r.ossrs.net&port=80&autostart=true&vhost=r.ossrs.net&schema=http)





## 使用SRS搭建





### SRS简介

`SRS`（`Simple-RTMP-Server`）

`SRS`是一个开源的直播服务器，它不仅仅是[RTMP](http://blog.fpliu.com/it/network/protocol/RTMP)服务器， 还是[HLS](http://blog.fpliu.com/it/network/protocol/HLS)服务器，还是[HTTP-FLV](http://blog.fpliu.com/it/network/protocol/HTTP-FLV)服务器， 还可以是[HTTP](http://blog.fpliu.com/it/network/protocol/HTTP)服务器。

[官网](https://ossrs.net/srs.release/releases/)



### 搭建RTMP服务器



#### 启用服务

启动Docker版本的SRS服务（[教程](https://github.com/ossrs/srs-docker#usage)）

```bash
echo "Run SRS 3.0 in docker" && docker run -p 1935:1935 -p 1985:1985 -p 8080:8080 -v /Users/yanghaibin/Downloads/IPC/srs/log/yours.log:/usr/local/srs/objs/srs.log ossrs/srs:3
```



##### 确定推流地址

```java
rtmp://127.0.0.1/live/tangqingsong
```



##### 使用OBS推流

![img](https://user-gold-cdn.xitu.io/2019/12/13/16eff205ad5092e2?imageslim)



回到OBS的主界面，点击开始推流按钮。



使用VLC来拉流。

```
rtmp://127.0.0.1/live/tangqingsong
```



[参考](https://juejin.im/post/5df37c136fb9a0164a10c571#heading-8)



### 搭建HTTP-FLV-TODO



[教程](https://github.com/ossrs/srs/wiki/v2_CN_DeliveryHttpStream#about-http-flv)

https://github.com/ossrs/srs/wiki/v1_CN_Home







## 在Mac上搭建



[参考](https://www.jianshu.com/p/cf74a34af15d)



### 安装

```
brew tap denji/nginx 
brew install nginx-full --with-rtmp-module 
nginx
```



在浏览器地址栏输入：[http://localhost:8080](http://localhost:8080/)



基本配置

- nginx配置文件所在位置 /usr/local/etc/nginx/nginx.conf

- nginx服务器根目录所在位置 /usr/local/var/www



### 配置

打开配置文件 `/usr/local/etc/nginx/nginx.conf`



```
http {
    ……
}

#在http节点下面(也就是文件的尾部)加上rtmp配置：
rtmp {
    server {
        listen 1935;
        application abcs {
            live on;
            record off;
        }
    }
}
```



```bash
$ nginx -s reload
```



### 通过ffmepg命令进行推流

```bash
ffmpeg -re -i /Users/yanghaibin/Downloads/IPC/rtmp/MindGame.mp4 -vcodec copy -f flv rtmp://localhost:1935/abcs/room
```



```cpp
ffmpeg -re -i /Users/xu/Desktop/bangbangbang.mp4 -vcodec libx264 -acodec aac -f flv rtmp://localhost:1935/rtmplive/home
```

- acc：RTMP的音频格式
- flv： RTMP的视频格式
- r：以本地帧频读数据，主要用于模拟捕获设备。表示ffmpeg将按照帧率发送数据，不会按照最高的效率发送



### 验证视频

使用VLC打开

```c++
rtmp://localhost:1935/abcs/room
```



本地地址

rtmp://172.16.1.123:1935/abcs/room



## HTTP FLV服务器



[nginx-http-flv-module](https://github.com/winshining/nginx-http-flv-module)



[搭建 基于nginx-http-flv-module的服务，通过flv.js进行网页直播](https://blog.csdn.net/rain_meter/article/details/88127209)



https://blog.csdn.net/rain_meter/article/details/88127209



SRS



## Nginx命令



启动nginx服务
`sudo brew services start nginx`

关闭nginx服务
`sudo brew services stop nginx`





查看Nginx的版本号：nginx -V

启动Nginx：start nginx

快速停止或关闭Nginx：nginx -s stop

正常停止或关闭Nginx：nginx -s quit

配置文件修改重装载命令：nginx -s reload





## 编译Nginx



[参考](https://aijishu.com/a/1060000000021573)



### 依赖

- PCRE——支持正则表达式。 是 Nginx 的核心和重写模块的所需依赖库。

```
wget https://ftp.pcre.org/pub/pcre/pcre-8.43.tar.bz2
tar zxf pcre-8.43.tar.bz2
```

- zlib——支持标头压缩。是 Nginx 的 Gzip 模块所需。

```
wget http://zlib.net/zlib-1.2.11.tar.gz
tar zxf zlib-1.2.11.tar.gz
```

- OpenSSL——支持HTTPS协议。是Nginx的SSL模块和其他模块所需。

```
wget http://www.openssl.org/source/openssl-1.1.1c.tar.gz
tar zxf openssl-1.1.1c.tar.gz
```



### 下载源



到这里选择版本：

http://nginx.org/download/

```
wget https://nginx.org/download/nginx-1.17.4.tar.gz
tar zxf nginx-1.17.4.tar.gz 
cd nginx-1.17.4
```



### 配置构建选项



```
./configure --add-module=/Users/yanghaibin/Downloads/nginx-module/nginx-http-flv-module-1.2.7 --with-http_ssl_module --with-pcre=../pcre-8.43 --with-zlib=../zlib-1.2.11 --with-openssl=../openssl-1.1.1c
```



### 编译



```
sudo make && sudo make install
```

