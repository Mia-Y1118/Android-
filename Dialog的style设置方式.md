### 一、Dialog与DialogFragment的使用场景

1、Dialog需要手动处理屏幕翻转的情况

- 在onCreate()中进行初始化，setContentView来加载布局。
- 可以使用构造函数进行传值

2、DialogFragment不需要进行任何处理，FragmentManager会自动管理DialogFragment的声明周期

- 传值需要通过Bundle传值，一般在onCreateView中进行值的接收，onAttach()中进行一些Listener的强制类型的转换

- 复写onCreateDialog创建DialogFragment : 一般用于创建替代传统的Dialog对话框的创景，UI简单，功能单一

- 复写onCreateView方法来创建DialogFragment:一般用于创建复杂内容的弹窗或全屏展示效果的场景，UI复杂，功能复杂，一般有网络请求等异步操作。

  ```java
  /**
       * setStyle() 的第一个参数有四个可选值：
       * STYLE_NORMAL|STYLE_NO_TITLE|STYLE_NO_FRAME|STYLE_NO_INPUT
       * 其中 STYLE_NO_TITLE 和 STYLE_NO_FRAME 可以关闭标题栏
       * 每一个参数的详细用途可以直接看 Android 源码的说明
       */
  setStyle(DialogFragment.STYLE_NO_TITLE, R.style.CustomDialog);
  
  
  // 设置宽度为屏宽、位置靠近屏幕底部
  Window window = dialog.getWindow();
  window.setBackgroundDrawableResource(R.color.transparent);
  WindowManager.LayoutParams wlp = window.getAttributes();
  wlp.gravity = Gravity.BOTTOM;
  wlp.width = WindowManager.LayoutParams.MATCH_PARENT;
  wlp.height = WindowManager.LayoutParams.WRAP_CONTENT;
  window.setAttributes(wlp);  
  ```

  

3、Dialog的style的设置

- 从下到上：

  ```ja
  <style name="shareDialogTheme" parent="android:Theme.Dialog">
          <item name="android:windowFrame">@null</item>
          <item name="android:windowFullscreen">true</item>
          <item name="android:windowNoTitle">true</item>
          <item name="android:windowBackground">@android:color/transparent</item>
          <item name="android:windowAnimationStyle">@style/mystyle</item>
          <item name="android:windowIsFloating">true</item>
          <item name="android:backgroundDimEnabled">true</item>
          <!-- 背景有灰度 -->
          <item name="android:windowContentOverlay">@null</item>
      </style>
      
      
        <style name="mystyle" parent="android:Animation">
          <item name="android:windowEnterAnimation">@anim/slide_from_bottom</item>
          <item name="android:windowExitAnimation">@anim/slide_out_bottom</item>
      </style>
      
  
  <?xml version="1.0" encoding="utf-8"?>
  <set android:interpolator="@android:anim/decelerate_interpolator"
  	xmlns:android="http://schemas.android.com/apk/res/android">
  	<translate android:duration="500"
  		android:fromYDelta="100.0%p" android:toYDelta="0.0" />
  </set>
  
  
  <?xml version="1.0" encoding="utf-8"?>
  <set xmlns:android="http://schemas.android.com/apk/res/android"
      android:interpolator="@android:anim/decelerate_interpolator" >
  
      <translate
          android:duration="500"
          android:fromYDelta="0.0"
          android:toYDelta="100.0%p" />
  
  </set>
  
  ```

  