---
layout: post
title:  "Android Data Binding"
date:   2020-02-17 16:57:48 +0800
categories: android jetpack databinding
---



####  LiveData和MutableLiveData

`LiveData`是不可变的，而`MutableLiveData`是可变的。

`MutableLiveData`扩展了`LiveData`并提供了`setValue()`和`postValue()`等方法。



#### Binding Adapter

DataBinding 有些不足，比如通过 Glide 这种第三方框架加载图片，还是得 `findViewById`。

DataBinding 提供了另一种解决思路：Binding Adapter

```java
@BindingAdapter({"imageRes"})
public static void loadImage(ImageView imageView, int resId) {
    Glide.with(imageView)
            .load(resId)
            .into(imageView);
}
```



```xml
<ImageView
    android:id="@+id/iv_avatar"
    imageRes="@{viewModel.avatar}"
    android:layout_width="50dp"
    android:layout_height="50dp"
    android:src="@mipmap/avatar_1" />
```

