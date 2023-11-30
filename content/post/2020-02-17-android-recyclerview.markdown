---
layout: post
title:  "Android RecyclerView和Paging"
date:   2020-02-22 16:57:48 +0800
categories: android recyclerview ui
---



## RecyclerView

参考工程：DemoRecyclerView1



框架**

```java
LetterAdapter mLetterAdapter = new LetterAdapter(characterList);
RecyclerView letterReView = findViewById(R.id.re_view);

letterReView.setAdapter(mLetterAdapter);
letterReView.setLayoutManager(new LinearLayoutManager(this, RecyclerView.VERTICAL, false));
```



**Adapter**

onCreateViewHolder() 创建每一行的Layout

onBindViewHolder() 为每一行Layout赋值

```java
private class LetterAdapter extends RecyclerView.Adapter<VH> {

    private List<Character> dataList;

    public LetterAdapter(List<Character> dataList) {
        this.dataList = dataList;
    }

    @NonNull
    @Override
    public VH onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        return new VH(LayoutInflater.from(parent.getContext()).inflate(R.layout.item_letter, parent, false));
    }

    @Override
    public void onBindViewHolder(@NonNull VH holder, int position) {
        Character c = dataList.get(position);
        holder.tv1.setText(c.toString());
        holder.tv2.setText(String.valueOf(Integer.valueOf(c)));
    }

    @Override
    public int getItemCount() {
        return dataList.size();
    }
}
```



**ViewHolder**

```java
private class VH extends RecyclerView.ViewHolder {
    TextView tv1;
    TextView tv2;

    public VH(@NonNull View itemView) {
        super(itemView);
        tv1 = itemView.findViewById(R.id.tv1);
        tv2 = itemView.findViewById(R.id.tv2);
    }
}
```



导入数据源

```java
List<Character> characterList = new ArrayList<>();
for (char c = 'a'; c <= 'z'; c++) {
    characterList.add(c);
}

mLetterAdapter = new LetterAdapter(characterList);
```



## Paging-简易版本

参考工程：DemoPaging



绑定Adapter

```java
adapter = new ConcertAdapter();
recyclerView.setLayoutManager(new LinearLayoutManager(getActivity()));
recyclerView.setAdapter(adapter);
```



Concert是一个自定义的数据类。

通过LivePagedListBuilder创建：用LiveData来包裹的`LiveData<PagedList<Concert>>`。

```java
PagedList.Config config = new PagedList.Config.Builder()
        .setPageSize(INITIALLOADSIZE) // 分页加载的数量
        .setEnablePlaceholders(false) // 当item为null是否使用PlaceHolder展示
        .setInitialLoadSizeHint(INITIALLOADSIZE) // 预加载的数量, 与分页加载的数量成倍数关系
        .build();

LiveData<PagedList<Concert>> data = new LivePagedListBuilder(new DataSourceFactorty(), config).build();
```



LiveData监听数据变化，将`LiveData<PagedList<Concert>>`传入Adpater

```java
data.observe(getViewLifecycleOwner(), new Observer<PagedList<Concert>>() {
    @Override
    public void onChanged(@Nullable PagedList<Concert> concerts) {
        Log.d("ldo", "MainFragment.onChanged() " + concerts.size());
        //调用PagedListAdapter中的方法绑定
        // 只调用一次！！
        adapter.submitList(concerts);
    }
});
```

查看源代码，可以看到

PagedListAdapter.submitList(adpater)会调用

```java
if (mPagedList == null && mSnapshot == null) {
    // fast simple first insert
    mPagedList = pagedList; // 关键一步
}
```



PageList

**核心类**，它从数据源取出数据，同时，它负责控制 **第一次默认加载多少数据，之后每一次加载多少数据，如何加载**等等，并将数据的变更反映到UI上。

PagedList 通过 Datasource 加载数据， 通过 Config 的配置，可以设置一次加载的数量以及预加载的数量等。

除此之外，PagedList 还可以向 RecyclerView.Adapter 发送更新UI的信号。



Google对 **PagedListAdapter** 的职责定义的很简单，仅仅是一个被代理的对象而已，所有相关的数据处理逻辑都委托给了 **AsyncPagedListDiffer**：

```java
public abstract class PagedListAdapter<T, VH extends RecyclerView.ViewHolder>
        extends RecyclerView.Adapter<VH> {

    protected PagedListAdapter(@NonNull DiffUtil.ItemCallback<T> diffCallback) {
        mDiffer = new AsyncPagedListDiffer<>(this, diffCallback);
        mDiffer.mListener = mListener;
    }

    public void submitList(PagedList<T> pagedList) {
        mDiffer.submitList(pagedList);
    }

    protected T getItem(int position) {
        return mDiffer.getItem(position);
    }

    @Override
    public int getItemCount() {
        return mDiffer.getItemCount();
    }

   public PagedList<T> getCurrentList() {
        return mDiffer.getCurrentList();
    }
}
```

当数据源发生改变时，实际上会通知 **AsyncPagedListDiffer** 的 **submitList()** 方法通知其内部保存的 **PagedList** 更新并反映在UI上。



**执行顺序**

````bash
D/ldo: DataSourceFactorty.create()
D/ldo: PositionPageDataSource loadInitial() requestedLoadSize: 5  requestedStartPosition:0
D/ldo: getSubList() start: 0 size: 5
D/ldo: MainFragment.onChanged() 5
D/ldo: PositionPageDataSource loadRange() startPosition: 5 loadSize:5
D/ldo: getSubList() start: 5 size: 5
D/ldo: PositionPageDataSource loadRange() startPosition: 10 loadSize:5
D/ldo: getSubList() start: 10 size: 5
D/ldo: PositionPageDataSource loadRange() startPosition: 15 loadSize:5
D/ldo: getSubList() start: 15 size: 5
````





## Paging-更完整的版本



框架图

![img](https://upload-images.jianshu.io/upload_images/9271486-9e49790f92f2ff1f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/png)



使用网络请求来获取信息，非常完整的骨架。

参考工程：AndroidPaging

参考的文章：[链接](https://blog.csdn.net/future234/article/details/83036188)



代码执行顺序

```bash
2020-02-22 23:26:30.233 31362-31362/com.xianlai.mypaging D/ldo: [main] RedditApi() create
2020-02-22 23:26:30.445 31362-31362/com.xianlai.mypaging D/ldo: [main] RedditViewModel() repoResult Transformations.map
2020-02-22 23:26:30.446 31362-31362/com.xianlai.mypaging D/ldo: [main] InMemoryByPageKeyRepository() postOfSubreddit
2020-02-22 23:26:30.451 31362-31362/com.xianlai.mypaging D/ldo: [main] RedditViewModel() posts Transformations.switchMap
2020-02-22 23:26:30.453 31362-31409/com.xianlai.mypaging D/ldo: [pool-1-thread-1] RedditDataSourceFactory() create
2020-02-22 23:26:30.471 31362-31409/com.xianlai.mypaging D/ldo: [pool-1-thread-1] PageKeyedSubredditDataSource() loadInitial
2020-02-22 23:26:33.815 31362-31409/com.xianlai.mypaging D/ldo: java.util.ArrayList
2020-02-22 23:26:33.817 31362-31362/com.xianlai.mypaging D/ldo: [main] RedditViewModel() model.posts.observe
2020-02-22 23:29:04.373 31362-31435/com.xianlai.mypaging D/ldo: [pool-1-thread-3] PageKeyedSubredditDataSource() loadAfter
2020-02-22 23:29:09.140 31362-31362/com.xianlai.mypaging D/ldo: [main] PageKeyedSubredditDataSource() loadAfter.onResponse
2020-02-22 23:29:10.266 31362-31439/com.xianlai.mypaging D/ldo: [pool-1-thread-4] PageKeyedSubredditDataSource() loadAfter
2020-02-22 23:29:11.320 31362-31362/com.xianlai.mypaging D/ldo: [main] PageKeyedSubredditDataSource() loadAfter.onResponse

```



官网标准的框架：[链接](https://developer.android.com/topic/libraries/architecture/paging#ex-observe-livedata)



另外一个[教程](https://www.jianshu.com/p/0b7c82a5c27f)



## 参考



[Paging原理分析](https://juejin.im/post/5c53ad9e6fb9a049eb3c5cfd#heading-17)

挺不错的