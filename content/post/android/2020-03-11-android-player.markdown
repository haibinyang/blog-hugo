---
layout: post
title:  "Android媒体播放器"
date:   2020-03-11 07:58:58 +0800
categories: android media player
---



## 总体



先学习框架

从最简单的MediaPlayer入门，做一个框架，



不不，直接从ExoPlayer入门，做一个基本框架，本地的视频播放器！！





## ExoPlayer



### 介绍

Youtube也使用这个组件。



### 版本和官网

- 版本：[2.11.3](https://github.com/google/ExoPlayer/blob/release-v2/RELEASENOTES.md)

- [官方网站](https://exoplayer.dev/)



### 模组

- `exoplayer-core`: Core functionality (required).
- `exoplayer-dash`: Support for DASH content.
- `exoplayer-hls`: Support for HLS content.
- `exoplayer-smoothstreaming`: Support for SmoothStreaming content.
- `exoplayer-ui`: UI components and resources for use with ExoPlayer.



还有一些[扩展](https://github.com/google/ExoPlayer/tree/release-v2/extensions/)：



### 媒体类型-TODO

- DASH
- HLS
- SmoothStreaming
- Progressive



### 简介

application level



### 入门



[CodeLab](https://codelabs.developers.google.com/codelabs/exoplayer-intro/?hl=zh-cn#2)：推荐







### 播放RTMP



添加依赖

```groovy
compile 'com.google.android.exoplayer:extension-rtmp:2.7.3'
```



使用RTMP的MediaSource

```java
RtmpDataSourceFactory rtmpDataSourceFactory = new RtmpDataSourceFactory();
MediaSource videoSource = new ExtractorMediaSource.Factory(rtmpDataSourceFactory)
  .createMediaSource(Uri.parse("rtmp://172.16.1.123:1935/abcs/room"));
```



[参考](https://medium.com/@KarthikPonnam/rtmp-player-android-using-exo-media-player-ac7a012b7a25)



分析

使用了[RTMP扩展](https://github.com/google/ExoPlayer/tree/release-v2/extensions/rtmp)

这个扩展其实使用的是Ant Media公司的[Librtmp库](https://github.com/ant-media/LibRtmp-Client-for-Android)



#### Librtmp

Librtmp是2019年5月发布的3.1.0的版本。



It compiles librtmp library without ssl.



## 扩展

Exoplayer支持扩展，支持ffmpeg来decode。

https://exoplayer.dev/supported-formats.html

https://github.com/google/ExoPlayer/tree/release-v2/extensions/ffmpeg

https://exoplayer.dev/demo-application.html



## G.711

暂时没找到支持的地方

https://exoplayer.dev/doc/reference/com/google/android/exoplayer2/audio/WavUtil.html

https://sourcegraph.com/github.com/google/ExoPlayer/-/blob/RELEASENOTES.md



自己做了一个

output_g711.mp4

没播放出声音。





### 其它·

学习曲线比较陡峭。



















