---
layout: post
title:  "协程-Coroutines"
date:   2020-02-23 16:57:48 +0800
categories: coroutines kotlin

---

## 历史



#### 并发

一开始大家想要同一时间执行那么三五个程序，大家能一块跑一跑。特别是UI什么的，别一上计算量比较大的玩意就跟死机一样。于是就有了**并发**，从程序员的角度可以看成是多个独立的逻辑流。内部可以是多cpu并行，也可以是单cpu时间分片，能快速的切换逻辑流，看起来像是大家一块跑的就行。



#### 进程

但是一块跑就有问题了。我计算到一半，刚把多次方程解到最后一步，你突然插进来，我的中间状态咋办，我用来储存的内存被你覆盖了咋办？所以跑在一个cpu里面的并发都需要处理上下文切换的问题。**进程**就是这样抽象出来个一个概念，搭配虚拟内存、进程表之类的东西，用来管理独立的程序运行、切换。



后来一电脑上有了好几个cpu，好咧，大家都别闲着，一人跑一进程。就是所谓的**并行**。

#### 内核

因为程序的使用涉及大量的计算机资源配置，把这活随意的交给用户程序，非常容易让整个系统分分钟被搞跪，资源分配也很难做到相对的公平。所以核心的操作需要陷入内核(kernel)，切换到操作系统，让老大帮你来做。

#### 线程

有的时候碰着I/O访问，阻塞了后面所有的计算。空着也是空着，老大就直接把CPU切换到其他进程，让人家先用着。当然除了I\O阻塞，还有时钟阻塞等等。一开始大家都这样弄，后来发现不成，太慢了。为啥呀，一切换进程得反复进入内核，置换掉一大堆状态。进程数一高，大部分系统资源就被进程切换给吃掉了。后来搞出**线程**的概念，大致意思就是，这个地方阻塞了，但我还有其他地方的逻辑流可以计算，这些逻辑流是共享一个地址空间的，不用特别麻烦的切换页表、刷新TLB，只要把寄存器刷新一遍就行，能比切换进程开销少点。

#### 用户态线程

如果连时钟阻塞、 线程切换这些功能我们都不需要了，自己在进程里面写一个逻辑流调度的东西。那么我们即可以利用到并发优势，又可以避免反复系统调用，还有进程切换造成的开销，分分钟给你上几千个逻辑流不费力。这就是**用户态线程**。

#### 协程

从上面可以看到，实现一个用户态线程有两个必须要处理的问题：

一是碰着阻塞式I\O会导致整个进程被挂起；

二是由于缺乏时钟阻塞，进程需要自己拥有调度线程的能力。如果一种实现使得每个线程需要自己通过调用某个方法，主动交出控制权。那么我们就称这种用户态线程是协作式的，即是**协程**。



## 基础

### 本质

- 协程是基于线程的，可以理解为在用户层使用一个线程模拟多线程操作。
- 避免了线程间的切换问题，大量节省资源。



### 子程序



子程序，或者称为函数，在所有语言中都是层级调用，比如A调用B，B在执行过程中又调用了C，C执行完毕返回，B执行完毕返回，最后是A执行完毕。

所以子程序调用是通过`栈`实现的，一个线程就是执行一个子程序。

子程序调用总是一个入口，一次返回，调用顺序是明确的。

而协程的调用和子程序不同。



### 子程序和协程

> “子程序就是协程的一种特例。”

协程看上去也是子程序，但执行过程中，在子程序内部可中断，然后转而执行别的子程序，在适当的时候再返回来接着执行。

注意，在一个子程序中中断，去执行其他子程序，不是函数调用，有点类似CPU的中断。比如子程序A、B：

```python
def A():
    print '1'
    print '2'
    print '3'

def B():
    print 'x'
    print 'y'
    print 'z'
```

假设由协程执行，在执行A的过程中，可以随时中断，去执行B，B也可能在执行过程中中断再去执行A，结果可能是：

```bash
1
2
x
y
3
z
```



但是在A中是没有调用B的，所以协程的调用比函数调用理解起来要难一些。

看起来A、B的执行有点**像多线程**，但协程的特点在于是**一个线程执行**。



### 协程举例



```kotlin
    suspend fun fetchDocs() {
        val result = get("developer.android.com")
        show(result)
    }

    suspend fun get(url: String) = withContext(Dispatchers.IO) {
                ...
    }
```



![img](https://user-gold-cdn.xitu.io/2019/6/20/16b72f0821633f92?imageslim)



**调用栈**：每个线程有一个调用栈(call stack), Kotlin使用它来追踪哪个函数在执行和它的局部变量。

**Suspend函数**：当调用到`suspend`修饰的函数的时候，Kotlin需要追踪正在运行的协程而不是正在执行的函数。



1. 绿色线条表示一个`suspend`的标记，绿色上面的是协程，绿色下面的是一个正常的函数
2. Kotlin 像正常函数一样调用`fetchDocs()` 函数，在调用栈上加一个 entry，这里也存储着`fetchDocs()`函数的局部变量
3. 继续往下执行，直到找到另一个`suspend`函数的调用（这里指的是 `get()` 函数调用），这时候Kotlin要去实现`suspend`操作（将函数的状态从堆栈复制到一个地方，以便以后保存，所有`suspend`的协程都会被放在这里）
4. 然后调用`get()`函数，同样新建一个entry，当调用到`withContext()`（withContext函数被 suspend 修饰）的时候，同样 执行suspend操作（过程和前面一样）。**此时主线程里的所有协程都被 suspend，所以主线程可以做其他事情（执行 onDraw，响应用户输入）**
5. 等待几秒后，网络请求会返回，这时Kotlin会执行resume操作（获取保存状态并复制回来，重新放回到调用栈上），之后会正常往下执行，如果`fetchDocs()`发成错误，会在这里抛出异常



### 协程比线程的优点



#### 调度

- 线程在调度上肯定是优胜于协程的，毕竟是内核调度的。
- 线程的开销基本上都是MB级别的，协程的开销只是KB级别的。
- 多线程比，线程数量越多，协程的性能优势就越明显。



#### 不需要锁

- 不需要多线程的锁机制。
- 因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，只需要判断状态就好了，所以执行效率比多线程高很多。



#### 结合使用

因为协程是一个线程执行，那怎么利用多核CPU呢？

最简单的方法是多进程+协程，既充分利用多核，又充分发挥协程的高效率，可获得极高的性能。

<img src="http://5b0988e595225.cdn.sohucs.com/images/20180622/6765e36cc4604fba897976638af03524.jpeg" alt="img" style="zoom:50%;" />





## Kotlin



### 学习教程



[由浅入深的教程](https://kaixue.io/kotlin-coroutines-1/)

[官方教程](https://www.kotlincn.net/docs/reference/coroutines/coroutines-guide.html)

[比较形象的教程](https://juejin.im/post/5d0afe0bf265da1b7152fb00#heading-3)

[总结的教程](https://www.cnblogs.com/mengdd/p/kotlin-coroutines-basics.html)





### 库

`kotlinx.coroutines` 是由 JetBrains 开发的功能丰富的协程库。

Maven库：[链接](https://mvnrepository.com/artifact/org.jetbrains.kotlinx/kotlinx-coroutines-core)

当前版本：1.3.3

模块：[链接](https://kotlin.github.io/kotlinx.coroutines/)





### Gradle配置



Add dependencies (you can also add other modules that you need):

```groovy
dependencies {
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.3.3'
}
```

And make sure that you use the latest Kotlin version:

```groovy
buildscript {
    ext.kotlin_version = '1.3.61'
}
```







### 概念

#### 协程Builder

启动一个新的协程, 常用的主要有以下几种方式:

- `launch`
- `async`
- `runBlocking`

叫作构建协程的coroutine builder。

其中， `launch`, `async`都是`CoroutineScope`类型的扩展方法。



有了CoroutineScope之后，可以通过一系列的`Coroutine builders`来启动协程。

- launch 启动一个协程，返回一个`Job`，可用来取消协程；有异常直接抛出。
- async  启动一个带返回结果的协程，可以通过Deferred.await()获取结果；有异常并不会直接抛出，只会在调用 await 的时候抛出。
- withContext 启动一个协程，传入`CoroutineContext`改变协程运行的上下文。



当`launch`, `async`或`runBlocking`开启新协程的时候, 它们自动创建相应的scope。

```kotlin
launch { /* this: CoroutineScope */
}
```



#### Job



##### launch返回Job

`launch`返回`Job`, 代表一个协程, 我们可以用`Job`的`join()`方法来显式地等待这个协程结束:

```kotlin
fun main() = runBlocking {
    val job = GlobalScope.launch {
        // launch a new coroutine and keep a reference to its Job
        delay(1000L)
        println("World! + ${Thread.currentThread().name}")
    }
    println("Hello, + ${Thread.currentThread().name}")
    job.join() // wait until child coroutine completes
}
```

`Job`还有一个重要的用途是`cancel()`, 用于**取消**不再需要的协程任务。



##### Deferred

`async`开启线程, 返回`Deferred`, `Deferred`是`Job`的子类, 有一个`await()`函数, 可以返回协程的结果.

`await()`也是suspend函数, 只能在协程之内调用。

```kotlin
fun main() = runBlocking {
    // @coroutine#1
    println(Thread.currentThread().name)
    val deferred: Deferred<Int> = async {
        // @coroutine#2
        loadData()
    }
    println("waiting..." + Thread.currentThread().name)
    println(deferred.await()) // suspend @coroutine#1
}

suspend fun loadData(): Int {
    println("loading..." + Thread.currentThread().name)
    delay(1000L) // suspend @coroutine#2
    println("loaded!" + Thread.currentThread().name)
    return 42
}
```



#### Scope

作用：scope的主要作用就是记录所有的协程, 并且可以取消它们。



##### CoroutineScope

CoroutineScope是一个接口。

```kotlin
public interface CoroutineScope {
    public val coroutineContext: CoroutineContext
}
```



##### 父子关系

launch是CoroutineScope的扩展方法，第三个参数是一个Lamda表达式。

```kotlin
public fun CoroutineScope.launch(
    context: CoroutineContext = EmptyCoroutineContext,
    start: CoroutineStart = CoroutineStart.DEFAULT,
    block: suspend CoroutineScope.() -> Unit
): Job
```

这个Lamda表达式的Receiver是：CoroutineScope

```kotlin
launch { /* this: CoroutineScope */
}
```

例如，

```kotlin
fun main() = runBlocking<Unit> {
    val job = launch {

        // CoroutineScope
        // 调用CoroutineScope的launcher方法
        // 继承它的Context
        val j2 = this.launch {
            delay(1000L)
            println("done")
        }

        // 不用join
      	// 这个父Coroutines会等待上面的子Coroutines结束
        println("start")
    }

    job.join()
}
```

这样就形成一个父子关系。



```kotlin
fun main() = runBlocking {
    /* this: CoroutineScope */
    launch { /* ... */ }
    // the same as:
    this.launch { /* ... */ }
}
```

这个例子中`launch`所启动的协程 是 外部协程(`runBlocking`启动的协程)的child. 这种"parent-child"的关系通过scope传递: child在parent的scope中启动。



协程的父子关系有以下两个特性:

- 取消：父协程被取消时, 所有的子协程都被取消。
- 等待：父协程永远会等待所有的子协程结束。



##### 创建Scope

创建scope可以用工厂方法: `MainScope()`或`CoroutineScope()`。



```kotlin
package top.yhb123.coroutines

import kotlinx.coroutines.*

fun main() = runBlocking {
    // this: CoroutineScope
    launch(CoroutineName("C2")) {
        log("C2: begin")
        delay(200L)
        log("C2: end")
    }

    coroutineScope {
        // 创建一个协程作用域
        launch(CoroutineName("C3")) {
            log("C3: begin")
            delay(500L)
            log("C3: end")
        }

        log("Custom coroutine scope: begin")
        delay(100L)
        log("Custom coroutine scope: end") // 这一行会在内嵌 launch 之前输出
    }

    log("Coroutine scope is over") // 这一行在内嵌 launch 执行完毕后才输出
}

fun log(msg: String) = println("[${Thread.currentThread().name}] $msg")
```





```bash
[main @coroutine#1] Custom coroutine scope: begin
[main @C2#2] C2: begin
[main @C3#3] C3: begin
[main @coroutine#1] Custom coroutine scope: end // 只等了100毫秒
[main @C2#2] C2: end // 只等了200毫秒
[main @C3#3] C3: end // 只等了500毫秒
[main @coroutine#1] Coroutine scope is over // 被coroutineScope阻塞
```



###### coroutineScope()

`CoroutineScope()`创建的Block会阻塞当前的代码，完成后才能执行下一面的代码`println("Coroutine scope is over")` ，效果类似`jion()`。



```kotlin
package top.yhb123.coroutines6

import kotlinx.coroutines.*

fun main() = runBlocking {

    launch(newSingleThreadContext("my-coroutines")) {
        log("newSingleThreadContext: 1")

        coroutineScope {
            log("Custom coroutine scope: begin")
            delay(1000L)
            log("Custom coroutine scope: end")
        }

        // 被上面的Scope阻塞
        // 上面有点像join
        log("newSingleThreadContext: 2")
    }

    // 不被上面阻塞
    log("Coroutine scope is over")
    // 但会等上面的Scope结束才结束，JVM不会马上结束
}

fun log(msg: String) = println("[${Thread.currentThread().name}] $msg")
```

在相同的协程。

```bash
[main @coroutine#1] Coroutine scope is over
[my-coroutines @coroutine#2] newSingleThreadContext: 1
[my-coroutines @coroutine#2] Custom coroutine scope: begin
[my-coroutines @coroutine#2] Custom coroutine scope: end
[my-coroutines @coroutine#2] newSingleThreadContext: 2
```



###### 自定义Scope



```kotlin
package top.yhb123.coroutines6

import kotlinx.coroutines.*

fun main() {
    val viewModelJob = Job()
  	// 自定义协程的Job，Dispatcher，name
    val uiScope = CoroutineScope(Dispatchers.IO + viewModelJob + CoroutineName("C1"))

    uiScope.launch {
        log("uiScope.launch: begin")
        delay(2000L)
        log("uiScope.launch: end")
    }

    log("Waiting for coroutines...")
    Thread.sleep(1000L)
    
  	// 取消协程
    log("cancle coroutines")
    viewModelJob.cancel()

    Thread.sleep(2000L)
    log("JVM over")
}

fun log(msg: String) = println("[${Thread.currentThread().name}] $msg")
```



```bash
[main] Waiting for coroutines...
[DefaultDispatcher-worker-1 @C1#1] uiScope.launch: begin
[main] cancle coroutines
[main] JVM over
```



```
package top.yhb123.coroutines6

import kotlinx.coroutines.*

fun main() {
    val viewModelJob = Job()
    val uiScope = CoroutineScope(Dispatchers.IO + viewModelJob + CoroutineName("C1"))

    uiScope.launch(Dispatchers.Default) {
        log("uiScope.launch: begin")
        delay(2000L)
        log("uiScope.launch: end")
    }

    log("Waiting for coroutines...")
    Thread.sleep(1000L)

    log("cancle coroutines")
    viewModelJob.cancel()

    Thread.sleep(2000L)
    log("JVM over")
}
```



可以在launch中添加不同的参数：

```kotlin
package top.yhb123.coroutines6

import kotlinx.coroutines.*

fun main() {
    val viewModelJob = Job()
    val uiScope = CoroutineScope(Dispatchers.Default + viewModelJob + CoroutineName("C1"))

  	// launch引进不同的参数
    uiScope.launch(Dispatchers.IO + CoroutineName("D2")) {
        log("uiScope.launch: begin")
        delay(2000L)
        log("uiScope.launch: end")
    }

    log("Waiting for coroutines...")
    Thread.sleep(1000L)

    log("cancle coroutines")
    viewModelJob.cancel()

    Thread.sleep(2000L)
    log("JVM over")
}
```





###### supervisorScope

TODO

`coroutineScope`和`supervisorScope`可以用来在suspend方法中启动协程. Structured concurrency保证: 当一个suspend函数返回时, 它的所有工作都执行完毕.

它们两者的区别是: 当子协程发生错误的时候, `coroutineScope`会取消scope中的所有的子协程, 而`supervisorScope`不会取消没有发生错误的其他子协程.



###### MainScope()-TODO





CoroutineScope

```kotlin
public interface CoroutineScope {
    public val coroutineContext: CoroutineContext
}
```



```kotlin
public fun MainScope(): CoroutineScope = ContextScope(SupervisorJob() + Dispatchers.Main)
```





###### GlobalScope

`GlobalScope`启动的协程都是独立的, 它们的生命只受到application的限制. 即`GlobalScope`启动的协程没有parent, 和它被启动时所在的外部的scope没有关系.

`launch(Dispatchers.Default) { ... }`和`GlobalScope.launch { ... }`用的dispatcher是一样的.

`GlobalScope`启动的协程并不会保持进程活跃. 它们就像daemon threads(守护线程)一样, 如果JVM发现没有其他一般的线程, 就会关闭。



###### runBlocking和coroutineScope

相同点：等待其协程体以及所有子协程结束。 

区别：

- runBlocking 方法会阻塞当前线程来等待， 而 coroutineScope 只是挂起，会释放底层线程用于其他用途。 
- runBlocking 是**常规函数**，而 coroutineScope 是挂起函数。



runBlocking 函数不是用来当做普通协程函数使用的，它的设计目的主要是用来**桥接普通阻塞代码和挂起风格**的（suspending style）的非阻塞代码 ， 例如用在 main 函数中 ，或者用于测试用例代码中。





#### 结构化的并发

如何避免泄漏呢？这其实就是`CoroutineScope` 的作用。

通过`launch`或者`async`启动一个协程需要指定`CoroutineScope`，当要取消协程的时候只需要调用`CoroutineScope.cancel()` ，kotlin 会帮我们自动取消在这个作用域里面启动的协程。



结构化并发可以保证代码更加安全，避免了协程的泄漏问题：

- 当作用域被取消，里面所有的协程被取消，因而可以取消不再需要的任务。
- 当`suspend`函数返回，里面的工作能保证完成，因而可以追踪正在执行的任务。
- 当协程出错，调用者或者作用域会收到通知，从而可以进行异常处理。

> 这种利用scope将协程结构化组织起来的机制, 被称为"structured concurrency"。



#### Context

协程总是在一个`context`下运行, 类型是接口`CoroutineContext`。

协程的`context`是一个索引集合, 其中包含各种元素, 重要元素就有`Job`和`dispatcher`。

`Job`代表了这个协程。



#### Dispatchers和线程

> A dispatcher controls which thread runs a coroutine.

`CoroutineContext`由一组协程的配置参数组成，可以指定协程的名称，协程运行所在线程，异常处理等。

- `CoroutineName`(指定协程名称)
- `Job`（协程的生命周期，用于取消协程）
- `CoroutineDispatcher`，可以指定协程运行的线程；如果不明确指定dispatcher, 协程将会继承它被启动的那个scope的context(其中包含了dispatcher)。



##### 新建协程

`newSingleThreadContext`创建了一个线程来跑协程，返回`ExecutorCoroutineDispatcher`

```kotlin
fun newSingleThreadContext(name: String): ExecutorCoroutineDispatcher =
    newFixedThreadPoolContext(1, name)
```



##### 各种Dispatcher的区别

| **Dispatchers**        | **用途**                     | **使用场景**                                                |
| ---------------------- | ---------------------------- | ----------------------------------------------------------- |
| Dispatchers.Main       | 主线程、UI交互、执行轻量任务 | Call  suspend functions, Call UI functions, Update LiveData |
| Dispatchers.IO         | 网络请求、文件访问           | Database,  Reading/writing files, Networking                |
| Dispatchers.Default    | CPU密集型任务                | Sorting  a list, Parsing JSON, DiffUtils                    |
| Dispatchers.Unconfined | 不限制任何指定线程           | 限制恢复后的线程                                            |



#### 等待



##### 方式一

新启一个协程, 然后用`join`明确地挂起等待。



##### 方式二

如果不是GlobalScope产生的协程，父协程会等待。

```kotlin
fun main() = runBlocking<Unit> {
    // start main coroutine
    GlobalScope.launch {
        delay(1000L)
        println("World! + ${Thread.currentThread().name}")
    }
    // 不会等待
}
```

会等待。

```kotlin
fun main() = runBlocking {
    // this: CoroutineScope
    launch(CoroutineName("C2")) {
        log("C2: begin")
        delay(2000L)
        log("C2: end")
    }

    log("Coroutine scope is over") // 这一行在内嵌 launch 执行完毕后才输出
}

fun log(msg: String) = println("[${Thread.currentThread().name}] $msg")
```



##### 方式三

切换线程还可以用`withContext`, 可以在指定的协程context下运行代码, 挂起直到它结束, 返回结果。



```kotlin
package top.yhb123.coroutines5

import kotlinx.coroutines.*

fun main() = runBlocking {
    log("1")

    withContext(Dispatchers.IO) {
        log("2")
        delay(1000)
        log("3")
    } // 阻塞，此Block完成后才会执行下面的代码

    log("5")
}
```



```bash
[main @coroutine#1] 1
[DefaultDispatcher-worker-1 @coroutine#1] 2
[DefaultDispatcher-worker-1 @coroutine#1] 3
[main @coroutine#1] 5
```



#### 自定义协程的名字



```kotlin
launch(CoroutineName("C2")) {
    log("C2: begin")
    delay(200L)
    log("C2: end")
}
```


#### 选择不同的线程

```
fun main() = runBlocking<Unit> {
    launch {
        // context of the parent, main runBlocking coroutine
        println("main runBlocking      : I'm working in thread ${Thread.currentThread().name}")
    }
    launch(Dispatchers.Unconfined) {
        // not confined -- will work with main thread
        println("Unconfined            : I'm working in thread ${Thread.currentThread().name}")
    }
    launch(Dispatchers.Default) {
        // will get dispatched to DefaultDispatcher
        println("Default               : I'm working in thread ${Thread.currentThread().name}")
    }
    launch(newSingleThreadContext("MyOwnThread")) {
        // will get its own new thread
        println("newSingleThreadContext: I'm working in thread ${Thread.currentThread().name}")
    }
}
```





##### Android实践

在Android这种UI应用中, 比较常见的做法是, 顶部协程用`CoroutineDispatchers.Main`, 当需要在别的线程上做一些事情的时候, 再明确指定一个不同的dispatcher。



```kotlin
launch(Dispachers.Main) {              // 👈 在 UI 线程开始
    val image = getImage(imageId)
    avatarIv.setImageBitmap(image)     // 👈 执行结束后，自动切换回 UI 线程
}
//                               👇
fun getImage(imageId: Int) = withContext(Dispatchers.IO) {
    ...
}
```



#### 超时-TODO

[参考](https://www.kotlincn.net/docs/reference/coroutines/cancellation-and-timeouts.html)





### 其它



#### 参考

[参考1](https://www.liaoxuefeng.com/wiki/897692888725344/923057403198272)

[不错的入门教程](https://blog.csdn.net/zheng199172/article/details/88800275)

[参考2](https://www.zhihu.com/question/20511233)



https://juejin.im/post/5bd9b881f265da393e6c4b5b

https://juejin.im/post/5d0afe0bf265da1b7152fb00#heading-3

https://kaixue.io/kotlin-coroutines-1/

https://www.cnblogs.com/mengdd/p/kotlin-coroutines-basics.html

https://www.kotlincn.net/docs/reference/coroutines/coroutine-context-and-dispatchers.html





#### 打开调试模式



程序执行使用了 -Dkotlinx.coroutines.debug 的JVM 参数，输出如下所示：

```bash
[main @main#1] Started main coroutine
[main @v1coroutine#2] Computing v1
[main @v2coroutine#3] Computing v2
[main @main#1] The answer for v1 / v2 = 42
```



#### 显示当前的线程和协程



```kotlin
fun log(msg: String) = println("[${Thread.currentThread().name}] $msg")
```

打开调度模式，格式如下：

```bash
[main @coroutine#1] Hello,
[DefaultDispatcher-worker-1 @coroutine#2] World!
```

[线程名 @协程名#编号]



#### 自定义协程名

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    // this: CoroutineScope
    launch(CoroutineName("自定义协程名字")) {
        // 在 runBlocking 作用域中启动一个新协程
        delay(1000L)
        log("World!")
    }

    log("Hello,")
}
```



```bash
[main @coroutine#1] Hello,
[main @自定义协程名字#2] World!
```



## Android中的协程



@UiThread的作用？

Since `viewModelScope` has a default dispatcher of `Dispatchers.Main`, 





Room使用自己的dispatcher来确定查询运行在后台线程.
所以你的代码不应该使用`withContext(Dispatchers.IO)`, 会让代码变得复杂并且查询变慢.

更多内容可见: [Room 🔗 Coroutines](https://medium.com/androiddevelopers/room-coroutines-422b786dc4c5).



### 教程



[Coroutines 协程](https://www.cnblogs.com/mengdd/p/kotlin-coroutines-basics.html)

[Coroutines在Android中的实践](https://www.cnblogs.com/mengdd/p/kotlin-coroutines-in-Android.html)-非常不错的中文文章



官方文章：

[将 Kotlin 协程与架构组件一起使用](https://developer.android.com/topic/libraries/architecture/coroutines#livedata)

[利用 Kotlin 协程提升应用性能](https://developer.android.com/kotlin/coroutines)



协程的[CodeLab](https://codelabs.developers.google.com/codelabs/kotlin-coroutines/#8)，但感觉没什么用



[官方Sample](https://github.com/android/architecture-components-samples/tree/master/LiveDataSample)-TODO

[Coroutines On Android (part III): Real work](https://medium.com/androiddevelopers/coroutines-on-android-part-iii-real-work-2ba8a2ec2f45)- TODO

[Easy Coroutines in Android: viewModelScope](https://medium.com/androiddevelopers/easy-coroutines-in-android-viewmodelscope-25bffb605471) - TODO







### Activity/Fragment & Coroutines

方法1: 持有scope引用:

```kotlin
class Activity {
    private val mainScope = MainScope()
    
    fun destroy() {
        mainScope.cancel()
    }
}   
```



方法2: 实现接口:

```kotlin
class Activity : CoroutineScope by CoroutineScope(Dispatchers.Default) {
    fun destroy() {
        cancel() // Extension on CoroutineScope
    }
}
```



### ViewModel & Coroutines

方法1: 自己创建scope

```kotlin
private val viewModelJob = Job()

private val uiScope = CoroutineScope(Dispatchers.Main + viewModelJob)
```

在ViewModel被销毁的时候:

```kotlin
override fun onCleared() {
    super.onCleared()
    viewModelJob.cancel()
}
```



方法二：利用`viewModelScope`

```kotlin
class MainViewModel : ViewModel() {
    // Make a network request without blocking the UI thread
    private fun makeNetworkRequest() {
      
       // launch a coroutine in viewModelScope 
        viewModelScope.launch(Dispatchers.IO) {
            // slowFetch()
        }
      
    }

    // No need to override onCleared()
}
```



### LifecycleScope & Coroutines

每一个[Lifecycle](https://developer.android.com/topic/libraries/architecture/lifecycle)对象都有一个`LifecycleScope`.



```kotlin
activity.lifecycleScope.launch {}
fragment.lifecycleScope.launch {}
fragment.viewLifecycleOwner.launch {}
```



### LifecycleScope和ViewModelScope



但是LifecycleScope启动的协程却不适合调用repository的方法. 因为它的生命周期和Activity/Fragment是一致的, 太碎片化了, 容易被取消, 造成浪费.

设备旋转时, Activity会被重建, 如果取消请求再重新开始, 会造成一种浪费。



可以把请求放在ViewModel中, UI层重新注册获取结果. `viewModelScope`和`lifecycleScope`可以结合起来使用.

举例: ViewModel这样写:

```kotlin
class NoteViewModel: ViewModel {
    val noteDeferred = CompletableDeferred<Note>()
    
    viewModelScope.launch {
        val note = repository.loadNote()
        noteDeferred.complete(note)
    }
    
    suspend fun loadNote(): Note = noteDeferred.await()
}
```

而我们的UI中:

```kotlin
fun onCreate() {
    lifecycleScope.launch {
        val note = userViewModel.loadNote()
        updateUI(note)
    }
}
```

这样做之后的好处:

- ViewModel保证了数据请求没有浪费, 屏幕旋转不会重新发起请求.
- lifecycleScope保证了view没有leak.



以下是实例：

```kotlin
class MainActivity : AppCompatActivity() {
    val userViewModel: NoteViewModel by lazy {
        ViewModelProviders.of(this).get(NoteViewModel::class.java)
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        System.setProperty("kotlinx.coroutines.debug", "on")

        logg("onCreate()")

        lifecycleScope.launch {
            logg("lifecycleScope.launch: start")
            val note = userViewModel.loadNote()
            logg("lifecycleScope.launch: end " + note)
        }
    }

    override fun onStop() {
        super.onStop()
        logg("onStop()")
    }

    override fun onDestroy() {
        super.onDestroy()
        logg("onDestroy()")
    }

    fun logg(msg: String) {
        Log.d("HB", Thread.currentThread().name + " " + msg)
    }
}
```





```kotlin
class NoteViewModel : ViewModel() {
    val noteDeferred = CompletableDeferred<String>()

    init {
        viewModelScope.launch {
            logg("viewModelScope.launch: 1")
            val note = repositoryLoadNote()
            logg("viewModelScope.launch: 2")

            noteDeferred.complete(note)
            logg("viewModelScope.launch: 3")
        }
    }

    suspend fun loadNote(): String = noteDeferred.await()


    suspend fun repositoryLoadNote(): String = withContext(Dispatchers.IO) {
        logg("repositoryLoadNote: Start")
        delay(10_000L)
        logg("repositoryLoadNote: end")

        "hello"
    }

    fun logg(msg: String) {
        Log.d("HB", Thread.currentThread().name + " " + msg)
    }
}
```



输出

```bash
03-08 21:14:08.075 20307-20307/? D/HB: main onCreate()
03-08 21:14:08.105 20307-20307/? D/HB: main @coroutine#2 lifecycleScope.launch: 1
03-08 21:14:08.105 20307-20307/? D/HB: main @coroutine#3 viewModelScope.launch: 1
03-08 21:14:08.115 20307-20429/? D/HB: DefaultDispatcher-worker-1 @coroutine#3 repositoryLoadNote: Start

// 旋转屏幕
03-08 21:14:14.865 20307-20307/? D/HB: main onStop()
03-08 21:14:14.905 20307-20307/? D/HB: main onDestroy()
03-08 21:14:14.955 20307-20307/? D/HB: main onCreate()
03-08 21:14:14.955 20307-20307/? D/HB: main @coroutine#5 lifecycleScope.launch: 1
03-08 21:14:18.115 20307-20429/? D/HB: DefaultDispatcher-worker-1 @coroutine#3 repositoryLoadNote: end
03-08 21:14:18.115 20307-20307/? D/HB: main @coroutine#3 viewModelScope.launch: 2
03-08 21:14:18.115 20307-20307/? D/HB: main @coroutine#5 lifecycleScope.launch: 2
03-08 21:14:18.115 20307-20307/? D/HB: main @coroutine#3 viewModelScope.launch: 3

```



### 网络/数据库 & Coroutines



根据Architecture Components的构建模式:

- ViewModel负责在主线程启动协程, 清理时取消协程, 收到数据时用`LiveData`传给UI.
- Repository暴露`suspend`方法, 确保方法main-safe.
- 数据库和网络暴露`suspend`方法, 确保方法main-safe. Room和Retrofit都是符合这个pattern的.



Repository暴露`suspend`方法, 是主线程safe的, 如果要对结果做一些heavy的处理, 比如转换计算, 需要用`withContext`自行确定主线程不被阻塞.



#### Retrofit

Retrofit从2.6.0开始提供了对协程的支持.

定义方法的时候加上`suspend`关键字。



#### Room

Room从2.1.0版本开始提供对协程的支持. 具体就是DAO方法可以是`suspend`的.

Room使用自己的dispatcher来确定查询运行在后台线程.
所以你的代码不应该使用`withContext(Dispatchers.IO)`, 会让代码变得复杂并且查询变慢.

更多内容可见: [Room 🔗 Coroutines](https://medium.com/androiddevelopers/room-coroutines-422b786dc4c5).



#### Retrofit&Room

直接从 `Dispatchers.Main`调用Retrofit或Romm的suspend方法就行；

他们都有自己定义的Dispatcher，不用额外使用`withContext(Dispatchers.IO)`。



> Both Room and Retrofit make suspending functions **main-safe**.
>
> It's safe to call these suspend funs from `Dispatchers.Main`, even though they fetch from the network and write to the database.



> Both Room and Retrofit use a custom dispatcher and **do not use `Dispatchers.IO`.**
>
> Room will run coroutines using the default [query](https://developer.android.com/reference/androidx/room/RoomDatabase.Builder.html#setQueryExecutor(java.util.concurrent.Executor)) and [transaction](https://developer.android.com/reference/androidx/room/RoomDatabase.Builder.html#setTransactionExecutor(java.util.concurrent.Executor)) `Executor` that's configured.
>
> Retrofit will create a new `Call` object under the hood, and call [enqueue](https://square.github.io/retrofit/2.x/retrofit/retrofit2/Call.html#enqueue-retrofit2.Callback-) on it to send the request asynchronously.



### 暂停生命感知的协程



[参考](https://developer.android.com/topic/libraries/architecture/coroutines#suspend)



依赖

```
    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:1.3.4"
    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.3.4"

    implementation 'androidx.lifecycle:lifecycle-runtime-ktx:2.2.0'
```

kotlinx-coroutines-core: [1.3.4](https://mvnrepository.com/artifact/org.jetbrains.kotlinx/kotlinx-coroutines-core)

kotlinx-coroutines-android：[1.3.4](https://mvnrepository.com/artifact/org.jetbrains.kotlinx/kotlinx-coroutines-android)

lifecycle-runtime-ktx：[2.2.0](https://mvnrepository.com/artifact/androidx.lifecycle/lifecycle-runtime-ktx)



```kotlin
class MainActivity : AppCompatActivity() {
    init { // Notice that we can safely launch in the constructor of the Fragment.
        lifecycleScope.launch {

            whenStarted {
                logg("whenStarted： 1")
                // The block inside will run only when Lifecycle is at least STARTED.
                // It will start executing when fragment is started and
                // can call other suspend methods.
                val canAccess = withContext(Dispatchers.IO) {
                    checkUserAccess()
                }

                // When checkUserAccess returns, the next line is automatically
                // suspended if the Lifecycle is not *at least* STARTED.
                // We could safely run fragment transactions because we know the
                // code won't run unless the lifecycle is at least STARTED.
                logg("whenStarted： 2")
            }

            // This line runs only after the whenStarted block above has completed.
            logg("lifecycleScope.launch： done")
        }
    }

    suspend fun checkUserAccess() {
        logg("checkUserAccess: start")
        delay(10_000L)
        logg("checkUserAccess: end")
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        System.setProperty("kotlinx.coroutines.debug", "on")

        logg("onCreate()")
    }

    override fun onStart() {
        super.onStart()
        logg("onStart()")
    }

    override fun onStop() {
        super.onStop()
        logg("onStop()")
    }

    fun logg(msg: String) {
        Log.d("HB", Thread.currentThread().name + " " + msg)
    }
}
```



输出：

```bash
03-08 20:27:03.475 8653-8653/? D/HB: main onCreate()
03-08 20:27:03.475 8653-8653/? D/HB: main onStart()
03-08 20:27:03.475 8653-8653/? D/HB: main whenStarted： 1
03-08 20:27:03.485 8653-8719/? D/HB: DefaultDispatcher-worker-1 checkUserAccess: start

// 将App放到后台
03-08 20:27:06.195 8653-8653/? D/HB: main onStop()
03-08 20:27:13.485 8653-8740/? D/HB: DefaultDispatcher-worker-3 checkUserAccess: end

// App回到前台
03-08 20:27:41.555 8653-8653/? D/HB: main onStart()
03-08 20:27:41.555 8653-8653/? D/HB: main whenStarted： 2
03-08 20:27:41.555 8653-8653/? D/HB: main lifecycleScope.launch： done
```



### 与LiveData结合



[参考](https://developer.android.com/topic/libraries/architecture/coroutines#livedata)



添加依赖

```groovy
implementation 'androidx.lifecycle:lifecycle-livedata-ktx:2.2.0'
```



```kotlin
class MainActivity : AppCompatActivity() {

    val user: LiveData<String> = liveData {
        val data = databaseLoadUser() // loadUser is a suspend function.
        emit(data)
    }

    suspend fun databaseLoadUser(): String = withContext(Dispatchers.IO) {
        logg("databaseLoadUser: Start")
        delay(10_000L)
        logg("databaseLoadUser: end")

        "hello"
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        System.setProperty("kotlinx.coroutines.debug", "on")

        logg("onCreate()")

        user.observe(this, Observer {
            logg("Observer: " + it)
        })
    }

    fun logg(msg: String) {
        Log.d("HB", Thread.currentThread().name + " " + msg)
    }
}
```