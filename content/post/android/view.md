---
title: "View"
description: 
date: 2023-12-31T15:39:31+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---





# Surface、SurfaceTexture、Texture、TextureView、SurfaceView、GLSurfaceView之间的关系



![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231231140447.png)





| 生产者     | 消费者                                                       | View          |                                                              |
| ---------- | ------------------------------------------------------------ | ------------- | ------------------------------------------------------------ |
| MediaCodec | Surface<br>内存中的一段绘图缓冲区。                          | SurfaceView   | SurfaceView是一个有自己Surface的View；其缺点是不能做变形和动画。 |
|            | SurfaceTexture<br/>用来捕获视频流中的图像帧的，视频流可以是相机预览或者视频解码数据。 | TextureView   | TextureView是API 14（Android 4.0）添加进来的<br/>SurfaceTexture可以用作非直接输出的内容流，这样就提供二次处理的机会。与SurfaceView直接输出相比，这样会有若干帧的延迟。同时，由于它本身管理BufferQueue，因此内存消耗也会稍微大一些。<br/>TextureView专门用来渲染像视频或OpenGL场景之类的数据，而且TextureView只能用在具有硬件加速的Window中,如果使用的是软件渲染，TextureView什么也不显示。也就是说对于没有GPU的设备，TextureView完全不可用。 |
|            |                                                              | GLSurfaceView |                                                              |



![](https://cdn.jsdelivr.net/gh/haibinyang/img@main/picgo/20231231141154.png)



[参考](http://www.aman.site/2016/08/04/SufaceTexture_vs_sufaceView/)

[参考](https://juejin.cn/post/6967640096547962911)
