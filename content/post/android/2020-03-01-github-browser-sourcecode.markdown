---
layout: post
title:  "Github Browser Sample解析"
date:   2020-03-01 16:57:48 +0800
categories: android
---





## 网络请求



### Retrofit基础

使用接口

`https://jsonplaceholder.typicode.com/posts`



[参考](https://medium.com/%E5%B7%A5%E7%A8%8B%E5%B8%AB%E6%B1%82%E7%94%9F%E6%8C%87%E5%8D%97-sofware-engineer-survival-guide/retrofit-2-%E8%B5%B7%E6%89%8B%E5%BC%8F-212644f33a9a)



```kotlin
val clientBuilder = OkHttpClient.Builder()
        .connectTimeout(60, TimeUnit.SECONDS)
val retrofit = Retrofit.Builder()
        .baseUrl("https://jsonplaceholder.typicode.com")
        .addCallAdapterFactory(LiveDataCallAdapterFactory())
        .addConverterFactory(GsonConverterFactory.create())
        .client(clientBuilder.build())
        .build()
val service = retrofit.create(JsonPlaceholderService::class.java)


val call = service.posts()
call.enqueue(object : Callback<List<PostsResp>> {

    override fun onResponse(call: Call<List<PostsResp>>?, response: Response<List<PostsResp>>?) {
        Log.d("HB","onResponse()")
    }

    override fun onFailure(call: Call<List<PostsResp>>?, t: Throwable?) {
        Log.d("HB","onFailure()")
    }

})
```



### Retrofit 与 LiveData

[参考](https://juejin.im/post/5d56497f518825107c565d88)

引进

- ApiResponse.kt

- LiveDataCallAdapterFactory

- LiveDataCallAdapter



调整Service

```kotlin
interface JsonPlaceholderService {
    @GET("/posts")
  // 返回 LiveData
    fun posts(): LiveData<ApiResponse<List<PostsResp>>>
}
```



使用

```kotlin
val bannerList = service.posts()
bannerList.observe(this, Observer {
    Log.d("main", "res:$it")
})
```



## 流程



```kotlin
class AppModule {

    @Singleton
    @Provides
    fun provideGithubService(): GithubService {
        return Retrofit.Builder()
            .baseUrl("https://api.github.com/")
            .addConverterFactory(GsonConverterFactory.create())
      // 自定义工厂类
            .addCallAdapterFactory(LiveDataCallAdapterFactory())
            .build()
            .create(GithubService::class.java)
    }
}
```





## 数据类-Repo

```
package com.android.example.github.vo

Repo
```





### ApiResponse



不理解这个类的作用

```kotlin
/**
 * Common class used by API responses.
 * @param <T> the type of the response object
</T> */
@Suppress("unused") // T is used in extending classes
sealed class ApiResponse<T> {
    companion object {
        fun <T> create(error: Throwable): ApiErrorResponse<T> {
            return ApiErrorResponse(error.message ?: "unknown error")
        }

        fun <T> create(response: Response<T>): ApiResponse<T> {
            return if (response.isSuccessful) {
                val body = response.body()
                if (body == null || response.code() == 204) {
                    ApiEmptyResponse()
                } else {
                    ApiSuccessResponse(
                        body = body,
                        linkHeader = response.headers()?.get("link")
                    )
                }
            } else {
                val msg = response.errorBody()?.string()
                val errorMsg = if (msg.isNullOrEmpty()) {
                    response.message()
                } else {
                    msg
                }
                ApiErrorResponse(errorMsg ?: "unknown error")
            }
        }
    }
}
```



根据具体接口定义返回的类

```kotlin
data class RepoSearchResponse(
    @SerializedName("total_count")
    val total: Int = 0,

    @SerializedName("items")
    val items: List<Repo>
) {
    var nextPage: Int? = null
}
```



定义接口

```kotlin
interface GithubService {

    @GET("search/repositories")
    fun searchRepos(@Query("q") query: String): LiveData<ApiResponse<RepoSearchResponse>>
}
```





在AppModule中创建了实例。

```kotlin
class AppModule {

    @Singleton
    @Provides
    fun provideGithubService(): GithubService {
        return Retrofit.Builder()
            .baseUrl("https://api.github.com/")
            .addConverterFactory(GsonConverterFactory.create())
            .addCallAdapterFactory(LiveDataCallAdapterFactory())
            .build()
            .create(GithubService::class.java)
    }
}
```

传入具体的RepoRepository

```kotlin
class RepoRepository @Inject constructor(
        private val appExecutors: AppExecutors,
        private val db: GithubDb,
        private val repoDao: RepoDao,
  // 传入
        private val githubService: GithubService
) {
 fun search(query: String): LiveData<Resource<List<Repo>>> {

        return object : NetworkBoundResource<List<Repo>, RepoSearchResponse>(appExecutors) {

          // 使用
            override fun createCall() = githubService.searchRepos(query)

        }.asLiveData()

    }
}
```



## Dagger2使用

使用的是2.16的版本，感觉比较老。

```groovy
versions.dagger = "2.16"
```



这个ViewModel的RepoRepository是如何传入进来的，不懂。

这里的RepoRepository没有添加@Inject标识，也不在Module中。

```kotlin
class SearchViewModel @Inject constructor(repoRepository: RepoRepository) : ViewModel()
```





RepoRepository使用@Inject，可以被当成“依赖”

```kotlin
@Singleton
class RepoRepository @Inject constructor(
        private val appExecutors: AppExecutors,
        private val db: GithubDb,
        private val repoDao: RepoDao,
        private val githubService: GithubService
)
```

其中的参数，通过Module来提供。

```kotlin
@Module(includes = [ViewModelModule::class])
class AppModule {

    fun provideGithubService(): GithubService {
        return Retrofit.Builder()
            .baseUrl("https://api.github.com/")
            .addConverterFactory(GsonConverterFactory.create())
            .addCallAdapterFactory(LiveDataCallAdapterFactory())
            .build()
            .create(GithubService::class.java)
    }

    fun provideDb(app: Application): GithubDb {
        return Room
            .databaseBuilder(app, GithubDb::class.java, "github.db")
            .fallbackToDestructiveMigration()
            .build()
    }

    fun provideUserDao(db: GithubDb): UserDao {
        return db.userDao()
    }


    fun provideRepoDao(db: GithubDb): RepoDao {
        return db.repoDao()
    }
}
```







## 工具类



### NetworkBoundResource-关键

> A generic class that can provide a resource backed by both the sqlite database and the network.







### Resource

A generic class that holds a value with its loading status.

```kotlin
data class Resource<out T>(val status: Status, val data: T?, val message: String?) {
}
```



状态

```kotlin
enum class Status {
    SUCCESS,
    ERROR,
    LOADING
}
```



是一个VO类，用于界面使用。

```xml
<TextView
    app:visibleGone="@{searchResult.status == Status.SUCCESS &amp;&amp; searchResult.data.size == 0}"
    tools:layout_editor_absoluteY="247dp" />
```



### RateLimiter

Utility class that decides whether we should fetch some data or not.

