---
layout: post
title:  "设计模式"
date:   2020-02-29 16:57:48 +0800
categories: designpattern
---



## 工厂模式



[参考](https://juejin.im/entry/58f5e080b123db2fa2b3c4c6)

### 简单工厂模式



定义产品接口

```java
public interface Shape {
    void draw();
}
```



实现产品1

```java
public class CircleShape implements Shape {

    public CircleShape() {
        System.out.println(  "CircleShape: created");
    }

    @Override
    public void draw() {
        System.out.println(  "draw: CircleShape");
    }

}
```



实现产品2

```ava
public class RectShape implements Shape {
    public RectShape() {
       System.out.println(  "RectShape: created");
    }

    @Override
    public void draw() {
       System.out.println(  "draw: RectShape");
    }

}
```



定义工厂，返回不同的产品

```java
 public class ShapeFactory {
          public static final String TAG = "ShapeFactory";
          public static Shape getShape(String type) {
              Shape shape = null;
              if (type.equalsIgnoreCase("circle")) {
                  shape = new CircleShape();
              } else if (type.equalsIgnoreCase("rect")) {
                  shape = new RectShape();
              }
              return shape;
          }
   }
```



使用简单工厂

```java
    Shape shape= ShapeFactory.getShape("circle");
    shape.draw();
```





#### 使用Kotlin来实现



```kotlin
package com.agg.kotlinapplication
 
enum class ComputerType {
    PC, Server
}

interface Computer {
    val cpu: String
 
    companion object Factory {
        operator fun invoke(type: ComputerType): Computer {
            return when (type) {
                ComputerType.PC -> PC()
                ComputerType.Server -> Server()
            }
        }
    }
}
 
class PC(override val cpu: String = "Core") : Computer
 
class Server(override val cpu: String = "Xeon") : Computer
 

fun main(args: Array<String>) {
    println(Computer.Factory(ComputerType.PC).cpu) // Core
}
```



companion object Factory：通过伴生对象创建静态工厂方法 ，方便调用。

operator fun invoke：通过运算符重载，简化方法的调用。注：（）就是重载过的invoke方法。

> 重载 Computer.Factory() 中的 ()



[参考](https://blog.csdn.net/agg_bin/article/details/104331797)



### 工厂方法



定义产品接口

```java
public interface Reader {
    void read();
}
```



实现具体的产品1

```java
public class JpgReader implements Reader {
    @Override
    public void read() {
        System.out.print("read jpg");
    }
}
```



实现具体的产品2

```java
public class PngReader implements Reader {
    @Override
    public void read() {
        System.out.print("read png");
    }
}
```



定义工厂接口

```java
public interface ReaderFactory {
    Reader getReader();
}
```



实现产品1的工厂

```java
public class JpgReaderFactory implements ReaderFactory {
    @Override
    public Reader getReader() {
        return new JpgReader();
    }
}
```



实现产品2的工厂

```java
public class PngReaderFactory implements ReaderFactory {
    @Override
    public Reader getReader() {
        return new PngReader();
    }
}
```



使用工厂1创建产品1

```java
ReaderFactory factory=new JpgReaderFactory();
Reader  reader=factory.getReader();
reader.read();
```

