
## 1、对比v4包与app包下的Fragment，有哪些不同点，使用时该注意哪些?  

- ①android.support.v4.app.Fragment:V4的兼容性更强，其可以兼容到android:minSdkVersion="4",就是android的版本号是1.6.
android.app.Fragment的兼容性的最低版本是android:minSdkVersion="11",就是android的3.0版本
- ②在运用的时候倒入的包不同
- ③继承的父类不同，v4的Fragment的FragmentManager fm = getSupportFragmentManager(),并且当前的Activity需要继承自FragmentActivity.
android.app.Fragment是通过FragmentManager fm = getFragment(),并且当前的Activity继承自Activity即可。
- ④使用的时候：V4的Fragment可以通过静态加载的方法在布局文件中<fragment>来使用。
但是android.app.Fragment需要也能通过<fragment>标签来加载，需要在程序中动态的通过add,replace的方法来使用。但是其继承的是Activity.
- ⑤注意，不管用哪一个包下面的，都必须完全运用那个包下面的。
- ⑥对于v4,需要在project structure中的dependencies中添加v4的包。

## 2、为什么创建Fragment时传递参数要用setArguments() ##

	在导入V4的Fragment的时候是没有有参数的构造函数的，所以不能通过构造函数的方式去传递值。
	在Activity在异常情况下退出的时候，比如切换横屏幕，竖屏幕的时候,会将临时数据存放在Bundle中，如果不用setArguments的时候，则会出现数据丢失，切换屏幕之后就无法显示了。

## 3、 将Fragment添加到Activity中的方式有哪些，有何区别。 ##

	静态添加：将Fragment作为一种组件，在布局文件中进行添加，不过要指明android：name,即将Fragment放到这个布局中，这样的话，在静态添加的fragment中就不能实现fragment的切换，替代等。
	
	动态添加：需要在布局文件中有一个Fragment的容器。在动态添加的时候，首先需要一个Fragment管理器，即FragmentManager,在android.app.Fragment中直接通过：FragmentManager fm = getFragmentManager(),而在v4的包中需要通过FragmentManager fm = getSupportManager()的形式获取。再在拿到FragmentManager的基础之上，得到FragmentTransication,来对Fragment来做一系列的操作。

	FragmentManager fragmentManager = getFragmentManager(); FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();

## 4、FragmentTransaction里面一些基本方法说明，比如add()/show()
这些方法是在动态加载Fragment中，对Fragment做的一系列的事务性操作 ##

	add():向Activity中添加一个Fragment，有三个重载的方法。
	remove():从Activity中移除一个Fragment
	replace():使用一个Fragment替换当前的一个Fragment
	hide():隐藏当前的Fragment,不会调用生命周期的方法
	show():显示之前隐藏的Fragment,不会调用生命周期的方法
	detach():会将View从UI中移除，detach()之后可以调用attach()来进行恢复
	attach():重建View视图，附加到UI上并显示

## 5、动态添加add()方法，各参数的意义，tag作用？ ##
	(findFragmentByTag)
	有三个重载的add()方法：
	
	add(Fragment,Tag):Tag的作用就是可以通过getFragmentManager.findFragmentByTag的方式来获取一个fragment,然后把它当成一个view来操作。（无UI布局，onCreateView是无效的）

	add(id,Fragment):注意前面的id是一个ViewGroup的id,后面是一个fragment的对象。

	add(id,Fragment,Tag):当Fragment没有UI的时候，可以通过findFragmentByTag来找到这个Fragment,这里不需要在Fragment中写onCreateView的方法，通过add(fragment,Tag)的方式去添加Fragment.

## 6、FragmentTransaction,一次事务中可以进行多个操作，比较commit()、commitNow()、commitAllowingStateLoss(),区别? ##

	事物的提交方式有4种，commit(),commitAllowingStateLoss(),commitNow(),commitNowAllowingStateLoss()
	
	commit():在主线程中异步执行，需要等待主线程的调度，其在使用的时候FragmentManager会检查是否已经保存了其状态，如果状态已经保存了，则就抛出IllegalStateException异常。就是说commit()需要在宿主Activity保存状态之前调用，否则就会报错。（因为如果Activity出现异常需要恢复状态，在保存状态之后的commit()将会丢失，这和调用的初衷不相符合。
	
	commitAllowingStateLoss():也是一个异步操作，但是其允许Activity保存状态之后调用，也就是说遇到状态丢失不会报错。（在状态出错是可以接受的情况下使用）
	
	commitNow():是同步执行的操作，需要在Activity执行保存状态之前来进行操作。否则会抛出异常。（FragmentManager.excutePendingTranscantions()也表示一下执行提交操作，但是其会把所有的任务一次性提交，这样有可能会有副作用）遇到状态丢失不会报错。
	
	commitNowAllowingStateLoss():是一个同步操作，但是允许在Activity保存状态之后调用。

## 7、 AllowingState原因，解决AllowingState异常有哪些方法 ##

	当Activity异常销毁的时候会调用onSaveInstanceState(),来保存状态信息，但是如果commit在保存状态信息之后操作，那么久相当于给Activity重新添加了一个fragment,这个时候的fragment的状态和上一个的fragment的状态不一样，所以就会报状态不正确的异常。可以通过commitAllowingState的方式。
## 8、onSaveInstanceState()、onPause()、onStop()三者的生命周期顺序? ##

- onSaveInstanceState()并不是每一次Activity的生命周期都会调用的，其只是用于在异常情况下销毁Activity的时候调用，保存的是和当前Activity状态相关的临时数据。
- onPause(),onStop()也可以用于保存一些数据，但是保存的都是持久化的数据。
- 若启动ActivityB之后，选择点击Home按钮，程序退到后台，那么执行顺序是：B：onPause -> B：onSaveInstanceState -> B：onStop。
程序在后台的时候，选择主动杀死程序进程，然后再从桌面点击应用启动，会显示之前的ActivityB，执行顺序是：B：onCreate -> B：onStart –> B：onRestoreInstanceState - > B：onResume。
- onSaveInstanceState()在Activity的onPause()的情况后再调用。
- onRestoreSaveInstanceState()在Activity的onResume之前调用。

## 9、Fragment事务中加不加addToBackStack()的区别，popBackStack()？ ##

	addToBackStack()的用途是是否将一次的事务操作加入到回退栈，官方的说法是是否要在回退的时候显示上一个Fragment.
	addToBackStack()保存的是一系列针对一个fragmentTransaction的操作记录，表示是否将上一次的Fragment加入到回退栈中，如果加入，则按back键的时候，前面的Fragment就会显示出来。如果不加入，前面的fragment就不会显示出来。
	popBackStack();　　弹出堆栈中的一个并且显示，也就是代码模拟按下返回键的操作。
## 10、 Fragment向Activity传递数据、Activity向Fragment传递数据、Fragment之间通信 ##

- Fragment之间的通信：

	可以通过Activity这个中间媒介来实现。在一个Fragment中通过Activity（getActivity()方法来获得Activity的实例）获取到另外一个Fragment,再调用另外一个Fragment的相关方法来实现通信。
	Fragment向Activity传递数据：

    通过接口的方式，在外部声明一个接口，在Fragment的Attach()的方法中去实例化Listener（该接口强转为activity类型便可和Activity有交际），在Fragment的onCreateView的方法中调用接口中的返回发，并放置相应的参数。接口的实现方法在Activity中来实现，拿到参数做一些与Activity更新界面相关的事情。

- Activity向Fragment中传递数据：

	 1、setArguments(Bundle)的方式去传递数据，然后在Fragment的onCreateView的方法中去getArgments()获取Bundle.
	
	 2、在Activity中定义返回某个需要传递参数的值，然后在Fragment的onAttach()方法中去调用这个方法，取到这个值。

## 11、DialogFragment ##
        ①可以有类似于Fragment一样的生命周期，便于管理
        ②DialogFragment可以自定义显示的布局，与普通的Dialog不一样，（普通的Dialog只是通过builder的方式去构建对话框）
        ③一般的Dialog在切换屏幕（Activity异常销毁再重建）的时候不会保存状态。
        ④在DialogFragment在切换屏幕的时候会保存状态。
- DialogFragment与Activity的交互的生命周期的调用：注意Dialog的onSaveInstanceState是在Activity的onSaveInstanceState之前。
- 在异常情况下，Fragment和Activity都会重建，这个时候的fragment比Activity先重建（即Fragment的onCreate在Activity的onCreate之前执行。
- 在onRestoreInstanceState的时候，这个方法主要是在Fragment和Activity的onStart之后，onResume之前执行的。
- 

## 12、ViewPager+Fragment相关 ##
	一般用到上述的时候需要在Fragment左右滑动的时候去切换Fragment.
	首先要在布局文件中加一个组件ViewPager的组件，这里需要导入的是v4下的包，主要是在使用Adapter的时候用的是FragmentPagerAdapter来关联ViewPager和Fragment,这个Adapter继承自ViewPager,在V4的包下。
	在Activity中获取到ViewPager组件，再把Fragment添加进去。
	可以对ViewPager设置监听滑动，setOnPageChanged(..)

## 13、 FragmentPagerAdapter和FragmentStatePagerAdapter的区别 ##
- FragmentPagerAdapter:在Fragment较少的情况下使用，主要是会把Fragment加入到内存，占用内存，Fragment不在viewPager.setOffscreenPageLimit（）的保护的范围内会调用FragmentManager的detach()方法，相应的Fragment的onDestroyView会执行，但Fragment实例仍在

- FragmentStatePagerAdapter:在Fragment较多的情况下使用，不会把Fragment加载到内存，当其中一个Fragment划过去的时候，这个Fragment就会被销毁，当需要可见的时候再重新创建这个fragment的实例。

## 14、懒加载 ##
	懒加载的意思就是需要的时候才去加载
	在ViewPager需要包含多个Fragment的时候，其在第一个Fragment创建的时候，其他的Fragment也相应的创建好了，及走了onAttach,onCreate,onCreateView,onActivityCreate,onStart,,onResume.Fragment的 onResume()表示的是当前Fragment处于可见且可交互状态，但由于ViewPager的缓存机制，它已经失去了意义，也就是打开第一个Fragment，但其实其他的Fragment的数据也都已经加载好了。如果加载数据的操作都比较耗时或者都是类似图片的占用大量内存，这时就应该考虑想想是否该实现懒加载。

	懒加载的实现方式：通过setVisible的方式，当用户对于当前的Fragment可见的时候去加载。如果可见，则再在onCreateView中去请求数据，初始化界面等操作。

## 15、 setUserVisibleHint调用时机 ##
	在切换Fragment的时候会调用，当fragment即将可见（在onAttach()之前），和即将不可见的时候就会调用。主要是设置一个标志位，用于懒加载。

