### 一、什么是内存泄漏？内存泄漏的结果？

##### 1、内存泄漏：一个不用的对象持续占有内存，使得对象的内存得不到及时的释放，从而造成内存空间的浪费。

- Java内存分配策略

  ```java
  静态存储区（方法区）：主要存放静态数据、全局 static 数据和常量。这块内存在程序编译时就已经分配好，并且在程序整个运行期间都存在。
  
  栈区：主要存放基本类型和对象的引用。当方法被执行时，存放方法体内的局部变量，并且在方法执行结束时释放局部变量所持有的内存。
  
  堆区：又称动态内存分配，通常就是指在程序运行时直接 new 出来的内存。这部分内存在不使用时将会由 Java 垃圾回收器来负责回收。
  ```

  

- Java垃圾回收机制:java虚拟机会不定时的回收不需要的对象占据的内存空间

  

- 怎么判断这个对象不再需要?

  ```java
  //没有引用与对象进行关联，java回收机制就很大可能将这个对象所占据的内存空间进行回收。
  new Person("lala")
    
  四种引用类型：
  //强引用：java垃圾回收机制不会回收强引用对象。(可能会导致内存泄漏)
  private Person people2 = new Person("lala");
  
  //软引用：在垃圾回收时，只有内存空间不足的时候，软引用对象的内存才会被回收
  SoftReference<Person> people = new SoftReference<Person>(Person("lala"));
  
  //弱引用：在垃圾回收时，不管当前内存空间是否足够，弱引用的内存都会被回收
  WeakReference<Person> people = new WeakReference<Person>(Person("lala"));
      
  //虚引用：和没有引用一样，任何时候都可能被回收。但是在回收虚引用对象内存之前，需要把虚引用加入到与之关联的引用队列中。
  ReferenceQueue<Person> referenceQueue = new ReferenceQueue<Person>();
  PhantomReference<Person> people = new PhantomReference<Person>(Person("lala"),reference);
  ```

  

##### 2、结果：内存泄漏严重时将导致内存溢出（OOM),最终导致应用程序crash。

```java
例子：当Activity的onDestory()方法被调用之后，正常情况下，Activity本身以及其涉及到的View,Bitmap等就应该被回收，但是如果某个后台线程持有对这个Activity的引用，那么Activity占据的内存就不能被回收，严重时将导致OOM，最终Crash。
```



### 二、内存泄漏分析工具

##### 1、MAT（Memory Analyzer Tool)

- 操作需要检查的应用程序，使用Android Studio自带的Memory Monitor生成操作过程中的hprof文件

![image-20190214165452410](https://raw.githubusercontent.com/Dammyouh/Android-/master/pictures/%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E6%80%BB%E7%BB%93%E5%9B%BE%E7%89%87/image-20190214165452410.png)

- 运用sdk中的platform-tools中的hprof-conv工具，将Android Studio 生成的hprof转换成一个标准格式的hprof文件

```java
hprof-conv [-z] <infile> <outfile>

例：./hprof-conv /Users/yangxiaoyu/Desktop/内存泄漏分析/智能练习.hprof /Users/yangxiaoyu/Desktop/内存泄漏分析/智能练习标准.hprof 

```

- 下载MAT(下载地址：https://www.eclipse.org/mat/)，并使用mat打开hpof文件

- 可能会遇到打不开MAT的问题，解决办法是通过执行命令行：

  ①：/Users/yangxiaoyu/Desktop/mat.app/Contents/MacOS/MemoryAnalyzer -data ./XXX执行。

  ②：双击Mat的MemoryAnalyzer进行运行

  

##### 2、Leak Canary(https://github.com/square/leakcanary)

```java
//导入第三方库
dependencies {
  debugImplementation 'com.squareup.leakcanary:leakcanary-android:1.6.3'
  releaseImplementation 'com.squareup.leakcanary:leakcanary-android-no-op:1.6.3'
}

//进行注册
public class BaseApplication extends Application {

  @Override public void onCreate() {
    super.onCreate();
    if (LeakCanary.isInAnalyzerProcess(this)) {
      // This process is dedicated to LeakCanary for heap analysis.
      // You should not init your app in this process.
      return;
    }
    LeakCanary.install(this);
  }
}
```



### 三、项目中存在的内存泄漏举例

##### 1、单例出现的内存泄漏

![leakCanary_list](https://raw.githubusercontent.com/Dammyouh/Android-/master/pictures/%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E6%80%BB%E7%BB%93%E5%9B%BE%E7%89%87/leakCanary_list.jpg)

![leakCanary_launchActivity](https://raw.githubusercontent.com/Dammyouh/Android-/master/pictures/%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E6%80%BB%E7%BB%93%E5%9B%BE%E7%89%87/leakCanary_launchActivity.jpg)

![项目内存中内存泄漏VipInfoModel](https://raw.githubusercontent.com/Dammyouh/Android-/master/pictures/%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E6%80%BB%E7%BB%93%E5%9B%BE%E7%89%87/leak_vipModel.jpg)

![项目中内存泄漏_launchActivity](https://raw.githubusercontent.com/Dammyouh/Android-/master/pictures/%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E6%80%BB%E7%BB%93%E5%9B%BE%E7%89%87/leak_launchActivity.jpg)



##### 2、Timer可能导致的内存泄漏

![Mat_NewVideoFloatFragment](https://raw.githubusercontent.com/Dammyouh/Android-/master/pictures/%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E6%80%BB%E7%BB%93%E5%9B%BE%E7%89%87/Mat_NewVideoFloatFragment.png)

![MAT_NewFloatView_section](https://raw.githubusercontent.com/Dammyouh/Android-/master/pictures/%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E6%80%BB%E7%BB%93%E5%9B%BE%E7%89%87/MAT_NewFloatView_section.jpeg)

![MAT_NewFloatFragment_root](https://raw.githubusercontent.com/Dammyouh/Android-/master/pictures/%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E6%80%BB%E7%BB%93%E5%9B%BE%E7%89%87/MAT_NewFloatFragment_root.png)

![code_NewVideoFloat_gc](https://raw.githubusercontent.com/Dammyouh/Android-/master/pictures/%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E6%80%BB%E7%BB%93%E5%9B%BE%E7%89%87/code_NewVideoFloat_gc.png)

- 原因：TimerTask对象在和Timer的schedule()方法配合使用的时候极容易造成内存泄露。这里内存泄漏在于Timer和TimerTask没有进行Cancel，从而导致Timer和TimerTask一直引用外部类Activity。
- 解决方案：在Fragment的onDetach()或者Activity的onDestroy()时进行Time和TimerTask的释放



### 四、常见的内存泄漏

- 持有Context引用造成的泄漏
- 线程之间通过Handler通信引起的内存泄漏
- 构造Adapter时，没有使用缓存的convertView
- 资源对象没关闭造成的内存泄露(Cursor、IO 流)
- 各种注册没取消
- static关键字的滥用
- WebView对象没有销毁
- GridView的滥用
- 线程的使用
- Bitmap的回收和置空(对象内存过大) 

