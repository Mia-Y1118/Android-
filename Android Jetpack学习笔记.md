## 一、Android Jetpack的作用

- 对代码的数据逻辑和UI界面深层解耦，实现数据驱动型的UI

  

## 二、Android Jetpack组件的优势

- 轻松管理应用程序的生命周期
- 构建可观察的数据对象，以便在基础数据库更新时通知试图
- 存储在应用程序轮换中未销毁的UI相关数据，在界面重建后恢复数据
- 轻松的实现SQLite数据库
- 系统自动调度后台任务的执行，优化使用性能
- 推荐使用的项目结构：
  - ![1542958266766](C:\Users\YANGXI~1\AppData\Local\Temp\1542958266766.png)

## 三、组件

#### 1、Lifecycler

```
Lifecycler:生命周期感知组件，执行操作以响应另一个组件的生命周期状态的更改。（可以舰艇活动组件生命周期的变			化，在每个生命周期执行相应的方法，不同于以往想在生命周期中执行相应的方法需要设置接口，然后在生命周		 期中回调接口，这样会造成代码的耦合，也会引发生命周期的问题）

Lifecycler的优点：
	1、实现了执行逻辑和活动的分离，代码解耦并且增加了代码的可读性
	2、在活动结束时自动移除监听，避免了生命周期的问题

```

#### 2、LiveData

```java
LiveData:可以实现当用户执行某正操作或服务器数据发生改变后，重新获取数据再次刷新界面的UI。解决了数据显示和刷		   新的问题。

LiveData刷新的使用：创建LiveData实例后，为可观察的数据添加观察者，在数据改变时会自动回调观察者。
    val nameObservable = Observer<String> {   // 创建观察者对象
        textView.text = it       // 定义onChange（）方法中的操作
    }
    
LiveData的原理：
	内部保存了LifecycleOwner和Observer，利用LifecycleOwner感知并处理声明中期的变化，Observer在数据改变		时遍历所有观察者并回调方法
	
LiveData的几点注意事项：
	①一个具有生命周期感知特性的可观察的数据保持类，使用LiveData保存数据时，在每次订阅或数据更新时会自动回调	设置的观察者从而更新数据，真正的实现了数据驱动的效果
	②LiveData 认为观察者的生命周期处于STARTED状态或RESUMED状态下，表示观察者处于活动状态，LiveData只通知	  活跃的观察者关于更新
	③LiveData会在活动处于Destroy时释放观察者，所以开发者无需特别处理
```

3、ViewModel:在活动重建时仍然保存数据，在活动创建完成从中获取数据

```java
ViewModel的生命周期：
①ViewModel对象的范围是在获取ViewModel时传递给ViewModelProvider的Lifecycle生命周期
②ViewModel在内存中知道Activity销毁或Fragment被移除
③系统首次调用活动对象的onCreate()方法时，通常会请求ViewModel
④系统可能会整个活动的整个生命周期中多次调用onCreate(),例如当设备屏幕旋转时。
⑤ViewModel从第一次请求ViewModel直到活动完成并销毁时存在

ViewModel的优点：
①数据和界面的分离，是数据驱动界面
②解决了运行中断和界面重建时的数据保存问题
③配合LiveData实时获取最新数据
④实现Activity中Fragment之间的数据交互

ViewModel的原理：
将数据保存到ViewModel中，然后为活动添加一个HolderFragment,HolderFragment中保存了ViewStore的实例，ViewStore中使用Map保存了ViewModel,从而在活动重新创建时获取原来的ViewModel.
```

4、paging:轻松地在应用程序RecyclerView中逐步和优雅地加载数据。

```java
Paging的好处：
①轻松流畅的实现下拉加载
②使用LiveData<PagedList>实现数据的可观察性
③配合Room实现数据库的加载和监听
④支持自定义DataSource

Paging实现的原理：
在PageListAdapter中显示数据，然后根据显示的position确定是否需要加载数据，当触发加载数据时，PagedList调用DataSource中配置的方法加载数据，并将数据传递到Adapter刷新UI
```

5、Room

```java
Room提供的三个主要的组件：
@Database:用来注解类，并且注解的类必须是继承自RoomDatabase的抽象类。
```

