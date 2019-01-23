## 一、Android Tools Attributes的应用

1、 xmlns:tools="http://schemas.android.com/tools"  可以在布局文件中实现实时预览的效果

2、去除Lint提示

```java
①tools:ignore
imageView : 一般需要加上 tools:ignore="contentDescription"，否则会报黄色警告。

②tools:tartgetApi
指定Api级别，可以使用数字Level,也可以直接使用API name,比如elevation属性只能在API21及更高版本使用，可以使用 tools:targetApi="LOLLIPOP" 忽略版本警告。

③tools:locale
```

3、布局预览

```java
①tools:context
指明与当前layout相关联的Activity,从而在预览时使用Activity主题，因为theme一般定义在Manifest文件中并且与activity联系在一起。

②tools:text
在design床就预览时可以看到TextView的text内容，而运行时则会被忽略。

③tools:layout 指定预览时用的layout布局文件
<fragment
	android:layout_width="match_parent"
	android:layout_height="match_parent"
	tools:layout="@layout/fragment_content"/>
	
④tools:listitem/listheader/listfooter  可用于listView来展示几个item
<android.support.v7.widget.RecyclerView
	android:layout_width="match_parent"
	android:layout_height="match_parent"
	tools:listitem="@android:layout/simple_list_item_checked"/>     

⑤tools:showIn 指定一个所在的布局文件，这样被嵌套的layout的design试图便可以预览嵌套一个include_content。

⑥tools:menu 指定layout预览时ActionBar展示的menu内容。

⑦tools:actionBarNavMode  用于指定layout预览时ActionBar的导航模式，值有standart,list,tabs三种。
```









