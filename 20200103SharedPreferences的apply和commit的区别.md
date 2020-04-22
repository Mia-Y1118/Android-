#### 一、SharePreference的基本使用

- ##### 获取sp对象

  ```java
  private var sp = context.getSharedPreferences(user_sp, Context.MODE_PRIVATE)
  ```

- ##### 通过editor对数据进行保存操作等

  ```java
  val edit = sp.edit()
  when (value) {
       is String -> edit.putString(key, value)
       is Int -> edit.putInt(key, value)
       is Long -> edit.putLong(key, value)
       is Float -> edit.putFloat(key, value)
       is Boolean -> edit.putBoolean(key, value)
  }
  //提交事务
  - ①edit.apply() ——>没有返回值
    
  - ②edit.commit() ——> 返回boolean
  ```

- ##### 获取数据

  ```java
  sp.getLong(key, 0L)
  ```

#### 二、commit 实现

![image-20200103161950921](/Users/yangxiaoyu/Library/Application Support/typora-user-images/image-20200103161950921.png)

- 先同步内存
- enqueueDiskWrite（），写入磁盘。postWriteRunnable 传的null.通常情况下在mDiskWritesInFlight（正在执行的写入磁盘操作的数量）为1的时候，直接在当前所在线程执行写入磁盘操作。否则还是异步到QueuedWork中去执行。**commit()时，写入磁盘操作会发生在当前线程的说法是不准确的。**

- 执行mcr.writtenToDiskLatch.await(); MemoryCommitResult 中有个一个CountDownLatch 成员变量，他的具体作用可以查阅其他资料。当前线程执行会堵塞在这，直到mcr.writtenToDiskLatch满足了条件。也就是当写入磁盘成功之后，会继续执行下面的操作

- **commit提交之后会有返回结果，同步堵塞直到有返回结果**(一般不需要返回结果，所以用apply)

  

#### 三、apply实现

- 加入到QueuedWork中，是一个单线程的操作。

- 没有返回结果。

- 默认会有100ms的延迟,延迟写入磁盘。

  

#### 四、commit 与apply的区别

- commit()方法是同步的有返回结果，同步保证使用Countdownlatch，即使同步但不保证往磁盘的写入是发生在当前线程的。
- apply()方法是异步的具体发生在QueuedWork中，里面维护了一个单线程去执行磁盘写入操作。

- commit()和apply()方法其实都是Block主线程。commit()只要在主线程调用就会堵塞主线程;apply()方法磁盘写入操作虽然是异步的，但是当组件(Activity Service BroadCastReceiver)这些系统组件特定状态转换的时候，会把QueuedWork中未完成的那些磁盘写入操作放在主线程执行，且如果比较耗时会产生ANR。

#### 五、总结

- 有问题，主线程堵塞。
- 效率低。
  - 1、优化方案
    - 低频 尽量保证多次edit一个apply,原因上文讲过，尽量维持低频的写入。
    - 异步 能用apply()方法提交的就用apply()方法提交，原因这个方法是异步的，有延迟的（100s）
    - 小量 尽量维持Sharepreferences的体量小些，方便磁盘快速写入。
    - 合规 如果存JSON数据，就不要使用Sharepreferences了，因为SharedPerences本质是xml文件格式存储的，要存储JSON文件需要转义效率很低。不如直接自己编写代码文件读写在App私有目录中存储。
  - 2、替代方案
    - 腾讯微信团队的MMKV采用内存映射的方式，解决SharedPreferences的各种问题。
    - 原理基于内存映射mmap

- 跨进程操作，需要借助Android平台常规的IPC手段（如，AIDL ContentProvider等）来完成。