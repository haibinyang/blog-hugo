---
layout: post
title:  "Android 库的版本"
date:   2020-02-20 16:57:48 +0800
categories: android lib version
---





## Project Gradle

```groovy
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    ext.kotlin_version = '1.3.50'
    repositories {
        // 镜像库
        maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
        maven { url 'https://maven.aliyun.com/repository/google' }

        google()
        jcenter()
        
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.5.3'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
      
      
        classpath "android.arch.navigation:navigation-safe-args-gradle-plugin:1.0.0"
    }
}

allprojects {
    repositories {
        // 国内镜像库
        maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
        maven { url 'https://maven.aliyun.com/repository/google' }

        google()
        jcenter()
        
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```



## App Gradle



[Jetpack的版本](https://developer.android.com/jetpack/androidx/versions?hl=zh-cn)



[Navigation](https://developer.android.com/jetpack/androidx/releases/navigation)



RecyclerView

```groovy
implementation 'androidx.recyclerview:recyclerview:1.1.0'
```