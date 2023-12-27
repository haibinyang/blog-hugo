---
layout: post
title:  "Android Architecture Blueprints v2"
date:   2020-03-05 16:57:48 +0800
categories: android
---



## 工程说明

[官网](https://github.com/android/architecture-samples)

[需求说明](https://github.com/android/architecture-samples/wiki/To-do-app-specification)



## 工程解析



TasksActivity解析



NavigationDrawer

NavController

Fragment

自动切换



### 文件结构

不好，建议参考Github Sample的文件结构。



## UI

### CoordinatorLayout-TODO



[链接](https://www.jianshu.com/p/640f4ef05fb2)

顶层的布局



### SwipeRefreshLayout



Google官方下拉刷新控件



### FloatingActionButton









## 基本配置



### 基本

Android Gradle Plugin：3.5.2

Gradle：5.4.1



最新版本是：[6.2.2](https://gradle.org/install/)



### Kotlin

当前最新版本：[1.3.70](https://mvnrepository.com/artifact/org.jetbrains.kotlin/kotlin-stdlib-jdk7)



BP使用：

```groovy
buildscript {
    ext.kotlinVersion = '1.3.61'
}
```



```groovy
  // Kotlin
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlinVersion"
```



### navigationVersion-TODO





### 使用 Java 8 语言功能



[链接](https://developer.android.com/studio/write/java8-support?hl=zh-cn)





## 第三方库



### 自动检测代码规范-TODO



[Spotless](https://github.com/diffplug/spotless)



[使用](https://blog.csdn.net/HinstenyHisoka/article/details/95165157)



### 日志：Timber



版本：4.7.1

[官网](https://github.com/JakeWharton/timber)

教程：见[链接](https://www.jianshu.com/p/4f54fcba3ad3)



#### 自定义输出格式

- 当前线程。
- 当前行号。

见[链接](https://stackoverflow.com/questions/38689399/log-method-name-and-line-number-in-timber)



[可能有用](https://www.jianshu.com/p/d4fb5d0fa26e)



打印Crash信息-TODO

https://github.com/JakeWharton/timber/blob/master/timber-sample/src/main/java/com/example/timber/ExampleApp.java



## 项目代码分析



### 线程执行器

建议参考Github，放到一个统一的类中。

```kotlin
class DefaultTasksRepository(
    private val tasksRemoteDataSource: TasksDataSource,
    private val tasksLocalDataSource: TasksDataSource,
    private val ioDispatcher: CoroutineDispatcher = Dispatchers.IO
) 
```





### 获取ViewModels

值得借鉴。



在Fragment中获取VM，需要指定泛型的类型<AddEditTaskViewModel>。

这样就解耦Fragment和VM工厂方法。

```kotlin
private val viewModel by viewModels<AddEditTaskViewModel> { getViewModelFactory() }
```

ViewModel 有依赖项，需要自定义工厂进行创建。

[参考](https://juejin.im/post/5d80920bf265da03b638e0bd)



#### VM的工厂方法

在Fragment的扩展方法，放在utils包下，用来创建对应的ViewModel，很方便。

```kotlin
fun Fragment.getViewModelFactory(): ViewModelFactory {
    val repository = (requireContext().applicationContext as TodoApplication).taskRepository
  
    return ViewModelFactory(repository, this)
}
```



使用一个辅助类：ViewModelFactory，生成不同的ViewModels工厂类。

根据泛型类型AddEditTaskViewModel，生成不同的ViewModels工厂类。

```kotlin
class ViewModelFactory constructor(
        private val tasksRepository: TasksRepository,
        owner: SavedStateRegistryOwner,
        defaultArgs: Bundle? = null
) : AbstractSavedStateViewModelFactory(owner, defaultArgs) {

    override fun <T : ViewModel> create(
            key: String,
            modelClass: Class<T>,
            handle: SavedStateHandle
      // 关键点
    ) = with(modelClass) {
        when {
            isAssignableFrom(StatisticsViewModel::class.java) ->
                StatisticsViewModel(tasksRepository)
            
            isAssignableFrom(TaskDetailViewModel::class.java) ->
                TaskDetailViewModel(tasksRepository)
        
            else ->
                throw IllegalArgumentException("Unknown ViewModel class: ${modelClass.name}")
        }
    } as T
    
}
```



下面说下Reposition的生命周期。



### Repository



#### Application

将Repository放在Application中。

```kotlin
class TodoApplication : Application() {

    // Depends on the flavor,
    val taskRepository: TasksRepository
        get() = ServiceLocator.provideTasksRepository(this)
}
```



Application从ServiceLocator中获取。



#### ServiceLocator

两个不同的Flavor

```groovy
productFlavors {
    mock {
        dimension "default"
        applicationIdSuffix = ".mock"
    }
    prod {
        dimension "default"
    }
}
```

每个Flavor对应一个ServiceLocator。



##### prod

```kotlin
object ServiceLocator {

    var tasksRepository: TasksRepository? = null

    fun provideTasksRepository(context: Context): TasksRepository {
        synchronized(this) {
            return tasksRepository ?: tasksRepository ?: createTasksRepository(context)
        }
    }
}
```



#### DataSource

定义接口：TasksDataSource

表示不同的数据来源：远程（Retrofit）、本地持久化（Room）和本地缓存（Memory）。



```kotlin
interface TasksDataSource {
    suspend fun saveTask(task: Task)
}
```



#### Repository

定义一个Reop接口：TasksRepository

```kotlin
interface TasksRepository {
    suspend fun saveTask(task: Task)
}
```



ServiceLocator创建基于TasksRepository的**DefaultTasksRepository**。

```kotlin
class DefaultTasksRepository(
    private val tasksRemoteDataSource: TasksDataSource,
    private val tasksLocalDataSource: TasksDataSource,
    private val ioDispatcher: CoroutineDispatcher = Dispatchers.IO
) : TasksRepository
```



不同的ServiceLocator传入不同的参数：tasksRemoteDataSource 和 tasksLocalDataSource。

例如，Prod：

Tasks**Remote**DataSource：指网络，这个项目是模拟网络请求。

tasks**Local**DataSource指：Room

```kotlin
private fun createTasksRepository(context: Context): TasksRepository {
    val newRepo =
        DefaultTasksRepository(TasksRemoteDataSource, createTaskLocalDataSource(context))
    tasksRepository = newRepo
    return newRepo
}
```



Remote：放在Prod的Flavor中。类包是：TasksRemoteDataSource

Local：放在主项目的local包中。类名是：Tasks**Local**DataSource



DefaultTasksRepository调用的其实是DataSource接口。

```kotlin
DefaultTasksRepository {
  override suspend fun saveTask(task: Task) {
      coroutineScope {
          launch {
              tasksRemoteDataSource.saveTask(task)
          }

          launch {
              tasksLocalDataSource.saveTask(task)
          }
      }
  }
}
```



#### DataSource



##### TasksLocalDataSource



Tasks**Local**DataSource需要DB的DAO接口，也需要Dispatcher。

这里使用了协程。

使用的是taskDao接口。

```kotlin
class TasksLocalDataSource internal constructor(
    private val tasksDao: TasksDao,
    private val ioDispatcher: CoroutineDispatcher = Dispatchers.IO
) : TasksDataSource {
  
    override suspend fun saveTask(task: Task) = withContext(ioDispatcher) {
        tasksDao.insertTask(task)
    }
  
}
```



##### TasksRemoteDataSource

```kotlin
    override suspend fun saveTask(task: Task) {
        TASKS_SERVICE_DATA[task.id] = task
    }
```



### 从UI到DataSource



#### 数据绑定



Activity

```kotlin
override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
): View? {
    val root = inflater.inflate(R.layout.addtask_frag, container, false)
  
  // 创建数据绑定
    viewDataBinding = AddtaskFragBinding.bind(root).apply {
      // Layout中的数据传入
        this.viewmodel = viewModel
    }
  
    return viewDataBinding.root
}
```



addtask_frag.xml文件

```xml
<layout>
    <data>
        <variable
            name="viewmodel" type="com.example.AddEditTaskViewModel" />
    </data>
```



使用viewmodel.dataLoading控制显示与否。

```xml
<com.example.android.architecture.blueprints.todoapp.ScrollChildSwipeRefreshLayout
    app:enabled="@{viewmodel.dataLoading}"
    app:refreshing="@{viewmodel.dataLoading}">
```



```xml
<ScrollView
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:visibility="@{viewmodel.dataLoading ? View.GONE : View.VISIBLE}">
```



双向绑定

```xml
<EditText
    android:id="@+id/add_task_title_edit_text"
    android:text="@={viewmodel.title}"/>

<EditText
    android:text="@={viewmodel.description}" />
```



响应点击事件

```xml
<com.google.android.material.floatingactionbutton.FloatingActionButton
    android:onClick="@{() -> viewmodel.saveTask()}" />
```



Layout --> ViewModel --> Reopository接口 --> DefaultTasksRepository --> 



#### 从UI到Repo到DataSource



定义一个实体类: Task

```kotlin
@Entity(tableName = "tasks")
data class Task @JvmOverloads constructor(
    @ColumnInfo(name = "title") var title: String = "",
    @ColumnInfo(name = "description") var description: String = "",
    @ColumnInfo(name = "completed") var isCompleted: Boolean = false,
    @PrimaryKey @ColumnInfo(name = "entryid") var id: String = UUID.randomUUID().toString()
)
```



定义一个Result类，封装：正常数据，异常和Loading状态。

```kotlin
sealed class Result<out R> {

    data class Success<out T>(val data: T) : Result<T>()
    data class Error(val exception: Exception) : Result<Nothing>()
    object Loading : Result<Nothing>()

    override fun toString(): String {
        return when (this) {
            is Success<*> -> "Success[data=$data]"
            is Error -> "Error[exception=$exception]"
            Loading -> "Loading"
        }
    }
}
```



开始，

在ViewModel中调用Repo，“同步”直接取结果。



tasksRepository返回的是一个密封类的Result，包含状态和数据。

```kotlin
viewModelScope.launch {
    tasksRepository.getTask(taskId).let { result ->
        // 判断成功与否                                
        if (result is Success) {
          // 取出数据
            onTaskLoaded(result.data)
        } else {
            onDataNotAvailable()
        }
    }
}
```



根据Result显示界面。

ViewModel相当于在控制UI界面。从这个角度看，VM承担了界面显示的职责。

```kotlin
// ViewModel
private fun onTaskLoaded(task: Task) {
    title.value = task.title
    description.value = task.description
    taskCompleted = task.isCompleted
    _dataLoading.value = false
    isDataLoaded = true
}

private fun onDataNotAvailable() {
    _dataLoading.value = false
}
```

--------------



Repo从DS中取结果

```kotlin
// Repo
override suspend fun getTask(taskId: String, forceUpdate: Boolean): Result<Task> {
        return tasksLocalDataSource.getTask(taskId)
}
```



DS获取数据，封装在Result的子类，返回结果。

```kotlin
// DS: Room
override suspend fun getTask(taskId: String): Result<Task> = withContext(ioDispatcher) {
    try {
        val task = tasksDao.getTaskById(taskId)
        if (task != null) {
            return@withContext Success(task)
        } else {
            return@withContext Error(Exception("Task not found!"))
        }
    } catch (e: Exception) {
        return@withContext Error(e)
    }
}
```





#### Event



自定义Event

```kotlin
open class Event<out T>(private val content: T) {
}
```



自定义Observer

```kotlin
class EventObserver<T>(private val onEventUnhandledContent: (T) -> Unit) : Observer<Event<T>> {
    override fun onChanged(event: Event<T>?) {
        event?.getContentIfNotHandled()?.let {
            onEventUnhandledContent(it)
        }
    }
}
```



在ViewModel添加属性

```kotlin
    private val _taskUpdatedEvent = MutableLiveData<Event<Unit>>()
    val taskUpdatedEvent: LiveData<Event<Unit>> = _taskUpdatedEvent
```



在Fragment添加观察者





```kotlin
viewModel.taskUpdatedEvent.observe(this, EventObserver {
    val action = AddEditTaskFragmentDirections
        .actionAddEditTaskFragmentToTasksFragment(ADD_EDIT_RESULT_OK)
    findNavController().navigate(action)
})
```



在ViewModel中广播事件

```kotlin
_taskUpdatedEvent.value = Event(Unit)
```



第二个实例

定义属性

```kotlin
private val _snackbarText = MutableLiveData<Event<Int>>()
val snackbarText: LiveData<Event<Int>> = _snackbarText
```



添加观察者

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



```kotlin
view?.setupSnackbar(this, viewModel.snackbarText, Snackbar.LENGTH_SHORT)
```



广播消息

```kotlin
_snackbarText.value = Event(R.string.empty_task_message)
```







### 其它



### 两个角度了解

添加数据

获取数据



## 不明白



```
private val args: AddEditTaskFragmentArgs by navArgs()
```

会自动生成：AddEditTaskFragmentArgs

```
args.taskId
```

如何定义里面的参数。

