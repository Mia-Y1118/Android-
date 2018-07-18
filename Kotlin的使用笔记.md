1、Kotlin的使用笔记

- 由于Kotlin与Databinding存在部分冲突，先不用Databinding模式，使用MVP模式

- 在声明变量的时候基本上是：private var mContext: Context? = null，布局文件和普通的一样

- 在变量的方法时需要在变量后面加上？

- 在触发点击事件的时候用

  ```java
   when (id) {
              R.id.iv_forward -> onForward()
              R.id.iv_download -> onDownload()
              R.id.iv_back -> finish()
              R.id.btn_continue_sign -> onResignClick()
          }
  ```

- Kotlin可以自动findViewById,所以在Activity中可以直接用Id进行Text等属性的设置，在ViewHolder中则用item.id.text形式

- 在写Adapter的时候，如果本地有需要用的变量，并且是通过构造传过来的，则可以在构造类的时候直接声明。