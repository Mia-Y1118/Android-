#### 一、smartRefreshLayout(<https://github.com/scwang90/SmartRefreshLayout>)的特点

- 支持[横向刷新](https://github.com/scwang90/SmartRefreshHorizontal)
- 支持多点触摸
- 支持淘宝二楼和二级刷新
- 支持嵌套多层的视图结构 Layout (LinearLayout,FrameLayout...)
- 支持所有的 View（AbsListView、RecyclerView、WebView....View）
- 支持自定义并且已经集成了很多炫酷的 Header 和 Footer.
- 支持和 ListView 的无缝同步滚动 和 CoordinatorLayout 的嵌套滚动 .
- 支持自动刷新、自动上拉加载（自动检测列表惯性滚动到底部，而不用手动上拉）.
- 支持自定义回弹动画的插值器，实现各种炫酷的动画效果.
- 支持设置主题来适配任何场景的 App，不会出现炫酷但很尴尬的情况.
- 支持设多种滑动方式：平移、拉伸、背后固定、顶层固定、全屏
- 支持所有可滚动视图的越界回弹
- 支持 Header 和 Footer 交换混用
- 支持AndroidX

#### 二、基本使用

- ##### 选择引用包说明：

  ```java
  - SmartRefreshLayout 刷新布局核心实现，自带ClassicsHeader（经典）、BezierRadarHeader（贝塞尔雷达）两个 Header.
  - SmartRefreshHeader 集成了各种Header
  - SmartRefreshFooter 集成了各种各种Footer
  ```

- ##### 在 build.gradle 中添加依赖

  ```java
  implementation 'com.scwang.smartrefresh:SmartRefreshLayout:1.1.0'  //1.0.5及以前版本的老用户升级需谨慎，API改动过大
  implementation 'com.scwang.smartrefresh:SmartRefreshHeader:1.1.0'  //没有使用特殊Header，可以不加这行
    
  //如果使用 AndroidX 在 gradle.properties 中添加
  android.useAndroidX=true
  android.enableJetifier=true
  ```

- ##### 使用默认Header,footer

  ```java
  1、布局文件添加：
  //SmartRefreshLayout直接包含一个组件，未设置则有默认的上拉加载Header(经典，转圈)
  <com.scwang.smartrefresh.layout.SmartRefreshLayout 
      android:id="@+id/refreshLayout"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
      <android.support.v7.widget.RecyclerView
          android:id="@+id/recyclerView"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:overScrollMode="never"
          android:background="#fff" />
  </com.scwang.smartrefresh.layout.SmartRefreshLayout>
  
  //如果只是需要有弹性效果，则添加，会自动不加入header。
  app:srlEnablePureScrollMode="true"
    
  2、代码设置监听：
  - setOnRefreshLoadMoreListener：上拉加载，下拉刷新
  - setOnLoadMoreListener：上拉加载
  - setOnRefreshListener： 下拉刷新
     final RefreshLayout refresh = root.findViewById(R.id.refreshLayout);
          refresh.setOnRefreshListener(new OnRefreshListener() {
              @Override
              public void onRefresh(@NonNull RefreshLayout refreshLayout) {
                  refresh.finishRefresh(100);
              }
          });
  
          refresh.setOnLoadMoreListener(new OnLoadMoreListener() {
              @Override
              public void onLoadMore(@NonNull RefreshLayout refreshLayout) {
                  refresh.finishLoadMore(500);
              }
          });
  
  ```

- ##### 使用自定义的Header

  ```java
  1、布局文件中进行设置:(HeaderView)
  <com.scwang.smartrefresh.layout.SmartRefreshLayout
      android:id="@+id/pull_view"
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  
      <com.sunland.core.ui.HeaderView
          android:layout_width="match_parent"
          android:layout_height="wrap_content"/>
  
      <androidx.recyclerview.widget.RecyclerView
          android:id="@+id/rv"
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:overScrollMode="never" />
  
  </com.scwang.smartrefresh.layout.SmartRefreshLayout>
  
  2、代码中进行设置
  pull_view.setRefreshHeader(PullHeader(PullHeaderView(context)))
    
  public RefreshLayout setRefreshHeader(@NonNull RefreshHeader header) {
       return setRefreshHeader(header, MATCH_PARENT, WRAP_CONTENT);
  }
  
  //自定义Header,需要实现RefreshHeader中的方法，可以实现继承InternalAbstract()，减少实现
  ```

  

- 其他属性设置

  ```java
  srlPrimaryColor	color	主题颜色
  srlAccentColor	color	强调颜色
  srlReboundDuration	integer	释放后回弹动画时长
  srlHeaderHeight	dimension	Header的标准高度
  srlFooterHeight	dimension	Footer的标准高度
  srlDragRate	float	显示拖动高度/真实拖动高度（默认0.5，阻尼效果）
  srlHeaderMaxDragRate	float	Header最大拖动高度/Header标准高度（默认2，要求>=1）
  srlFooterMaxDragRate	float	Footer最大拖动高度/Footer标准高度（默认2，要求>=1）
  srlEnableRefresh	boolean	是否开启下拉刷新功能（默认true）
  srlEnableLoadmore	boolean	是否开启加上拉加载功能（默认true）
  srlEnableHeaderTranslationContent	boolean	拖动Header的时候是否同时拖动内容（默认true）
  srlEnableFooterTranslationContent	boolean	拖动Footer的时候是否同时拖动内容（默认true）
  srlEnablePreviewInEditMode	boolean	是否在编辑模式时显示预览效果（默认true）
  srlDisableContentWhenRefresh	boolean	是否在刷新的时候禁止内容的一切手势操作（默认false）
  srlDisableContentWhenLoading	boolean	是否在加载的时候禁止内容的一切手势操作（默认false）
  ```

  