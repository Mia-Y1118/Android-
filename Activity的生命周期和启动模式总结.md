##一、Activity的生命周期：

###1、典型情况下Activity的生命周期（详见Activity的生命周期的图）：
	①onCreate:与onDestory相对应，表示Activity正在被创建，这是生命周期的第一个方法，主要做一些初始化的工作，比如用setContentView去加载布局资源，初始化Activity等。耗时的工作在异步线程上完成

	②onStart:与onStop相对应，表示Activity即将开始，这时的Activity已经可见，但是还没有出现在前台，还无法与用户交互。

	③onResume:表示Activity已经可以看见了，并且出现在前台，可以与用户产生交互了。

	④onPause:与onResume相对应，表示Activity正在暂停，正常情况下，onStop接着就会被调用。在特殊情况下，如果这个时候用户快速地再回到当前的Activity,那么onResume会被调用（极端情况）。一般来说，在这个生命周期状态下，可以做一些存储数据、停止动画的工作，但是不能太耗时，如果是由于启动新的Activity而唤醒的该状态，那会影响到新Activity的显示，原因是onPause必须执行完，新的Activity的onResume才会执行。（Activity此时是可见的：dialog,或者新Activity的主题是透明的）

	⑤onStop:表示Activity即将停止，已经不可见，同样不能做过多耗时操作。

	⑥onDestroy:表示Activity即将被销毁，这是Activity的最后一个生命周期，可以做一些回收工作和最终的资源释放。

	⑦onRestart:表示Activity正在重新启动。一般情况下，在当前Activity从不可见重新变为可见的状态时onRestart就会被调用。这种情形一般是由于用户的行为所导致的，比如用户按下Home键切换到桌面或者打开了一个新的Activity（这时当前Activity会暂停，也就是onPause和onStop被执行），接着用户有回到了这个Activity，就会出现这种情况。

###2、注意几点：
	①当用户按下Back键回退时，回调onPause,onStop,onDestroy

	②	HOME键的执行顺序：onPause->onStop->onRestart->onStart->onResume 
		BACK键的顺序： onPause->onStop->onDestroy->onCreate->onStart->onResume 

	③在ActivityA启动ActivityB的时候，需要activityA先执行onPause,ActivityB才会创建（onCreate,onStart,onResume)。

	④Activity的启动过程：简单的说就是请求Activity的请求是交给Instrumentation来处理，然后通过Binder向AMS发送请求，AMS内部维护一个ActivityStack并负责Activity的状态同步，AMS通过ActivityThread去同步Activity的状态而完成生命周期方法的调用。

###3、异常情况下Activity的生命周期（Activity被系统回收或者由于当前设备的configuration发生改变从而导致Activity被销毁重建)：

	①资源相关的系统配置发生改变时，Activity被杀死并重新创建（比如：Activity处于竖直状态，如果突然旋转屏幕，由于系统配置发生了改变，在默认情况下，Activity就会被销毁并且重新创建，当然可以组织系统重新创建新的Activity：android:configChanges="orientation|screenSize")，加上screenSize是因为minSdkVersion和targetSdkVersion有一个大于了13。

	②Activity在异常情况下被终止，系统会调用onSaveinstanceState来保存当前Activity的状态，并且调用时机是在onStop之前,与onPause没有特定的时序关系。正常情况下，不会调用这个方法。当新Activity被重新创建的时候，系统会调用onRestoreInstanceState来回复数据，在onstart之后执行onRestoreInstanceState。

	③资源内存不足导致低优先级的Activity被杀死：当系统内存不足时，系统就会按照上述优先级去杀死目标Activity所在的进程，并后续通过onSaveInstanceState和onRestoreInstanceState来存储和回复数据。

	④当Activity在异常情况销毁的时候，系统会默认保存当前Activity的视图结构，并且在Activity重启的时候为我们恢复这些数据（文本框的输入数据，ListView的滚动位置等）。保存和恢复View的层次结构，系统的工作流程如下：当Activity会调用onSaveInstanceState去保存数据，然后委托Window去保存数据，接着Window再委托它的上级容器去保存数据，顶级是一个Viewgroup,很可能是一个DecorView,然后顶级容器再去一一通知它的子元素去保存数据。


##二、Activity的启动模式（4种）

###1、standard:标准模式

	每次启动一个Activity都会重新创建一个新的实例，不管这个实例是否已经存在。并且谁启动了这个Activity,那么这个Activity就运行在启动它的这个Activity所在的栈中。

###2、singleTop:栈顶复用模式

	①如果新的Activity已经位于任务栈的栈顶，那么此Activity不会被重新创建，同时它的onNewIntent方法会被调用，通过此方法的参数，可以取出当前请求的信息。（onNewIntent)

	②如果新的Activity已经位于任务栈或者栈中不存在，但不是位于栈顶，则这个Activity会被重新创建（oncreate,onStart,onResume)。

###3、singleTask:栈内复用模式（单实例模式）

	①当一个具有singleTask模式的Activity A 请求启动，系统首先会寻找是否存在A想要的任务栈，如果不存在，就先创建一个A的任务栈,并将A的实例放入栈中。
	
	②如果存在A需要的任务栈，则首先判断是否存在A的实例，如果存在，则调用onNewIntent来唤醒A，并将处于A上面的Activity出栈(clearTop),使得A处于栈顶的位置。  如果不存在,则创建一个A的实例，放置于该栈的栈顶。

###4、singleInstance:单实例模式

	①具有此模式的Activity只能单独的位于一个任务栈中。

	②比如Activity A 是singleInstance模式,当A启动后,系统会为它创建一个新的任务栈。A独自运行在这个任务栈中，后续的请求均不会创建新的Activity,除非这个独特的任务栈被系统销毁了。

##三、TaskAffinity:主要和singleTask启动模式或者allowTaskReparenting属性配对使用,其他情况下没有意义。

	①当TaskAffinity和singleTask启动模式配对的时候，它是具有该模式Activity的目前任务栈的名字,待启动的Activity会运行在名字和TaskAffinity相同的任务栈中,Activity默认的任务栈的名称为包名。

	②当TaskAffinity和allowTaskReparenting配对的时候，当应用A启动应用B的Activity C后,当按下home键,再启动应用B时,如果Activity的allowTaskReparenting属性为true,C会从A的任务栈转移到B的任务栈中。

##四、如何给Activity指定启动模式：

	①在AndroidManifest中指定launchMode.(无法为Activity设置FLAG_ACTIVITY_CLEAR_TOP标识）

	②通过Intent(优先级更高,但是无法指定singleInstance模式):intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK)

##五、Activity常用的Flags

	①FLAG_ACTIVITY_NEW_TASK:作用是为Activity指定"singleTask"的启动模式

	②FLAG_ACTIVITY_SINGLE_TOP:作用是为Activity指定"singleTop"的启动模式

	③FLAG_ACTIVITY_CLEAR_TOP:具有此标志位的Activity,当它启动时,在同一个任务栈中所有位于它上面的Activity都要出栈，这个标志位一般会和singleTask启动模式一起出现。

	④FLAG_ACTIVITY_EXCLUDE_FROM_RECENTS:具有这个标志位的Activity不会出现在历史Activity的列表中,当某些情况,我们不需要通过历史列表回到我们的Activity的时候,这个标记比较有用。其等同于：android:excludeFromRecents = "true".


##六、显示Intent和隐式Intent:

	①显示Intent:（指明了要跳到哪个Activity或者Service，包括包名和类名)
	Intent intent = new Intent();
	intent.setAction(Activity1.this,Activity2.class)

	②隐式Intent:(没有指明要跳转的类）在AndroidManifest中的intent-filter中实现过滤。intentFilter的过滤信息有action,category,data.一个Activity可以有多个intent-filter,一个Intent只要能匹配任何一组intent-filter即可成功启动对应的Activity.

	③action的匹配规则：要求Intent的action存在且必须和过滤规则中的其中一个action相同，action区分大小写，大小写不同字符串相同的action会匹配失效。

	④category的匹配规则：category的匹配规则和action不一样,要求Intent中如果含有category,那么所有的category都必须和过滤规则中的某个category相同。（Intent可以不设置category,但是Intent必须设置action)

	⑤data的匹配规则：与action类似,如果Intent中定义了data,那么Intent中必须定义可匹配的data.
	data由两部分组成,mimeType(媒体类型)和URI(http://www.baidu.com:80/search/info)<shcheme>://<host>:<port>/[<path>|<pathPrefix>|<pathPattern>]。