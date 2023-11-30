---
layout: post
title:  "Android记录"
date:   2020-03-11 07:58:58 +0800
categories: android
---



## 经验



必须使用[视图绑定](https://developer.android.com/topic/libraries/view-binding)，但不一定要使用数据绑定



### 数据绑定



[关于Android databinding的调查总结](https://blog.csdn.net/awy1988/article/details/88543242)

[我不使用Android Data Binding的四个理由](https://mafei.me/2016/08/14/%E8%AF%91%E6%96%87-%E6%88%91%E4%B8%8D%E4%BD%BF%E7%94%A8Android-Data-Binding%E7%9A%84%E5%9B%9B%E4%B8%AA%E7%90%86%E7%94%B1/)



数据绑定使得 Bug 很难被调试。你看到界面异常了，有可能是你 View 的代码有 Bug，也可能是 Model 的代码有问题。数据绑定使得一个位置的 Bug 被快速传递到别的位置，要定位原始出问题的地方就变得不那么容易了。



数据双向绑定：不利于代码重用。客户端开发最常用的重用是View，但是数据双向绑定技术，让你在一个View都绑定了一个model，不同模块的model都不同。那就不能简单重用View了。





## 技巧





### 显示协程的名字



```kotlin
System.setProperty("kotlinx.coroutines.debug", "on")
```



### 支持Rtl-阿拉伯语

```xml
<application
    android:allowBackup="false"
    android:name=".TodoApplication"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:supportsRtl="true"
    android:theme="@style/AppTheme">
```



### Kotlin中的Fragment的方法和属性



view属性

```
public View getView() {
    return mView;
}
```



### 使用 Java 8 语言功能



[官网](https://developer.android.com/studio/write/java8-support?hl=zh-cn)



```groovy
android {
      ...
        
      // Configure only for each module that uses Java 8
      // language features (either in its source code or
      // through dependencies).
      compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
      }
  
      // For Kotlin projects
      kotlinOptions {
        jvmTarget = "1.8"
      }
    }
    
```



### APK signature scheme

单独使用V2签名的apk是不能在小于7.0的手机上安装的，会出现签名证书找不到的情况，为了防止出现这种情况，AS使用了可以同时选择两种签名方式
即：7.0以下使用V1的签名方式，7.0以后的就使用V2的签名方式

[参考](http://ddrv.cn/a/234695)

如果想关闭，

```groovy
  android {
    ...
    defaultConfig { ... }
    signingConfigs {
      release {
        storeFile file("myreleasekey.keystore")
        storePassword "password"
        keyAlias "MyReleaseKey"
        keyPassword "password"
        v2SigningEnabled false
      }
    }
  }
```



## 知识点



### 版本号

如何记忆：

- 21：5.0，20开始算
- 23：6.0
- 24：7.0，4*7=24
- 26：8.0
- 28：9.0



| **代号**    | **版本** | **API 级别** |
| ----------- | -------- | ------------ |
| Pie         | 9        | API 级别 28  |
| Oreo        | 8.1.0    | API 级别 27  |
| Oreo        | 8.0.0    | API 级别 26  |
| Nougat      | 7.1      | API 级别 25  |
| Nougat      | 7.0      | API 级别 24  |
| Marshmallow | 6.0      | API 级别 23  |
| Lollipop    | 5.1      | API 级别 22  |
| Lollipop    | 5.0      | API 级别 21  |



### 使用APK分析器来比较

[官网](https://developer.android.com/studio/build/apk-analyzer)



### 生成图标的网站

https://appicon.co/





## 问题



### Cleartext HTTP traffic to xxx not permitted



[参考](https://blog.csdn.net/xiaoqiang_0719/article/details/83374980)

针对这个问题，有以下三种解决方法：

（1）APP改用https请求

（2）targetSdkVersion 降到27以下

（3）更改网络安全配置



1.在res文件夹下创建一个xml文件夹，然后创建一个network_security_config.xml文件，文件内容如下：

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
  <base-config cleartextTrafficPermitted="true" />
</network-security-config>
```


2.接着，在AndroidManifest.xml文件下的application标签增加以下属性：

```xml
<application
...
 android:networkSecurityConfig="@xml/network_security_config"
...
  />
```

