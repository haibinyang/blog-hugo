---
layout: post
title:  "Android UI组件"
date:   2020-03-06 07:58:58 +0800
categories: android ui
---

## 

## Snackbar



Kotlin版本有个扩展方法`View.setupSnackbar()`

使用：直接绑定Snackbar和LiveData

```kotlin
private fun setupSnackbar() {
    view?.setupSnackbar(this, viewModel.snackbarText, Snackbar.LENGTH_SHORT)
}
```



ViewModel中的_snackbarText的定义如下：

```kotlin
    private val _snackbarText = MutableLiveData<Event<Int>>()
    val snackbarText: LiveData<Event<Int>> = _snackbarText
```



其中，Event类，一个Wrappter，包装LiveData

```kotlin
/**
 * Used as a wrapper for data that is exposed via a LiveData that represents an event.
 */
open class Event<out T>(private val content: T) {
```



`Event<Int>`中的Int是指String ID。



回到View.setupSnackbar的定义，要求`LiveData<Event<Int>>`



自定义ViewEx.kt，引进一个扩展程序：

```kotlin
fun View.setupSnackbar(
    lifecycleOwner: LifecycleOwner,
    snackbarEvent: LiveData<Event<Int>>,
    timeLength: Int
) {

    snackbarEvent.observe(lifecycleOwner, Observer { event ->
        event.getContentIfNotHandled()?.let {
            showSnackbar(context.getString(it), timeLength)
        }
    })
}
```



## RefreshLayout-TODO









