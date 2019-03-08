##一、Android PopupWindow的使用
#####1、PopupView的基本使用
```java
①PopupWindow这个类是用来实现一个弹窗,可以使用任意布局的View作为其内容,这个弹出框是悬浮在当前的Activity之上的。

②使用步骤：
//a.准备PopupWindow的布局View，即PopupWindow相当于一个布局View的容器。
View view = LayoutInflater.from(this).inflate(R.layout.popup,null)

//b.初始化PopupWindow的高度宽度等（一般都是包裹内容）,有多个构造函数,主要是设置popupWindow的大下。
PopupWindow popupWindow = new PopupWindow(ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT);

//c.设置PopupWindow的视图内容：(将布局文件装进PopupWindow的容器内）
popupWindow.setContentView(view)

//d.popupWindow的一些属性的设置：
popupWindow.setBackgroundDrawable();
popupWindow.setOutsideTouchable(true/false);默认false //设置外部点击是否消失
popupWindow.setFocusable(true/false) //设置返回键点击是否消失
popupWindow.serAnimationStyle(R.style.AnimDown);
设置消失监听：
popupWindow.setOnDismissListener....
popupWindow的弹出方式：
popupWindow.showAsDropDown(...);
```

#####2、使用PopupWindow中遇到的问题：
	①当两个按钮A,B均需要弹出PopupWindow并相互切换的时候,可能会出现当点击了A按钮，这个时候B的popupWindow消失，A按钮的PopupWindow并没有立马显示出来，需要再点击一下A，A的PopupWindow才显示出来。
	②解决方案是：
	在每个按钮弹出PopupWindow的时候,进行两外一个按钮的popUpWindow的空判断和是否显示的判断,手动地将其dismiss().

##二、对于PopupWindow的动画的设置

#####1、使用PopupWindow.setAnimationStyle(R.style.AnimDown)

#####2、对Style的使用：
	①在res下新进一个anim文件,里面主要放一些动画进入和动画出去的文件。
	②在Style中进行配置，如下：主要用于指定动画的名称,以及映射动画的进入退出文件。
	<style name="popwin_anim_style">
	    <item name="android:windowEnterAnimation">@anim/popupview_fade_in</item>
	    <item name="android:windowExitAnimation">@anim/popupview_fade_out</item>
	</style>
	③使用资源文件：通过R.style.popwin_anim_style

### 三、使用

```java
class SelectPackageWindow(private var mContext: Context,
                          private var listener: PackageWindowAdapter.OnPackageSelectListener?,
                          private var packageList: ArrayList<String>) : PopupWindow(mContext) {

    init {
        initView()
    }

    private fun initView() {
        val view = LayoutInflater.from(mContext).inflate(R.layout.select_package_window, null)
        contentView = view

        view.rv_package_list.layoutManager = LinearLayoutManager(mContext)
        view.rv_package_list.adapter = PackageWindowAdapter(mContext, listener, packageList)
        setBackgroundDrawable(mContext.resources.getDrawable(R.drawable.window_package_bg))
        //外部点击消失
        isOutsideTouchable = true
        //返回键点击消失
        isFocusable = true
    }

    fun showPopupWindow(parentView: View) {
        if (!isShowing) {
            showAsDropDown(parentView, -Utils.dip2px(mContext, 10f).toInt(), Utils.dip2px(mContext, 5f).toInt())
        }
    }

}
```

- 限制PopupWindow的最大高度：

```java
 window = SelectPackageWindow(act!!, this, packageList)
            if (packageList?.size ?: 0 > 5) {
                window?.width = Utils.dip2px(act, 158f).toInt()
                window?.height = Utils.dip2px(act, 290f).toInt()
            }
 window?.showPopupWindow(iv_course_icon)
```

