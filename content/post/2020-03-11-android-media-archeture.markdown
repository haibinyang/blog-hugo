---
layout: post
title:  "Android多媒体框架"
date:   2020-03-11 07:58:58 +0800
categories: android media
---



## 框架



从上往下

- MediaCodec
- StageFright API
- OpenOMX
- Driver





<img src="https://img.toutiao.io/c/8086781b97029a098d78fe92995cd166" alt="img" style="zoom:50%;" />





### MediaCodec

MediaCodec 是提供给上层应用的 Java 接口。

自 Android 4.1 （API 16) 引入的编解码接口，它是 Android 多媒体架构的一部分，通常和 MediaExtracto, MediaCrypto, Image, Surface, AudioTrack, MediaMuxer 一起使用。



### StageFright

StageFright 是 Android 平台预设的多媒体框架，自 Andorid 2.3 开始才被引入进来。

> 最早 MediaCodec 调用的多媒体框架是 OpenCore，OpenCore 的优点是跨平台的，但是由于过于庞大和复杂，自 Android 2.3 开始 StageFright 正式加入



### OpenMAX

> OpenMAX 全称是 Open Media Acceleration ( 开发多媒体加速器 )。

OpenMAX 为多媒体软硬开发提供了一套标准接口，OpenMAX 是为音视频，图像编解码而设计，许多嵌入式设备都使用了 OpenMAX 标准 ，比如 Android 平台。

OpenMAX 标准定义了 DL，IL, AL 层：

- AL ( Application Layer 应用层 )
- IL ( Integration Layer 整合层 )
- DL（ Devlopment Layer 开发层 ）



#### DL层

DL 层定义了音视频，图像处理接口，一般 DL 层由设备芯片厂商提供实现，并提供编解码器的功能。 在 Android 系统中，Google 提供了一些内置的软编解码器：

*1. OMX.google.h264.encoder,*

*2. OMX.google.h264.decoder,*

*3. OMX.google.acc.encoder,*

*4. OMX.google.acc.decoder*



如果手机厂商需要提供硬编解码器就需要实现 DL 层。

当设备芯片厂商开发完成编解码器后, 会将编解码器信息注册到 *system/etc/media_codecs.xml* 和 *system/etc/media_profiles.xml* 文件中，我们可以通过分析这两个文件获取当前设备所有的编解码器列表，解析解码器最大支持的视频宽高等信息。



可以通过adb获取

```bash
adb pull /system/etc/media_codecs.xml
adb pull /system/etc/media_profiles.xml
```





## 参考



[MediaCodec/OpenMAX/StageFright 介绍（Android）](MediaCodec/OpenMAX/StageFright 介绍（Android）)

