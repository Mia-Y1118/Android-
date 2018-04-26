## ViewRoot和DecorView ##
- ViewRoot对应于ViewRootImpl的实现类，是连接WindowManager和DecorView的纽带，View的三大流程（测量，布局，绘制）均是通过ViewRoot来完成的

- 绘制过程：从ViewRoot的performTraversals方法开始的，经过measure,layout,draw三个过程，最终将View绘制出来。performTraversals会依次调用performMeasure,performLayout,performDraw这三个方法来完成顶级View的measure，layout，draw.
 
- 具体：performMeasure中会调用measure方法,在measure中又会调用OnMeasure,在OnMeasure中会对所有的子元素进行measure。如果子元素也是一个ViewGroup,则会根据上面的步骤再次进行绘制。

-DecorView作为顶级View,一般会在内部包含一个竖直方向的LinearLayout,LinearLayout里面有两个部分,上面是标题栏，下面是内容栏，在Activity中通过setContentView设置的是内容栏。

## MeasureSpec ##
- MeasureSpec代表一个32位的int值,高2位代表SpecMode(测量模式）,低30位代表SpecSize（某种测量模式下的规格大小）.
- 在测量的过程中，系统会将View的LayoutParams根据父容器所施加的规则转换成对应的MeasureSpec,然后再根据这个measureSpec来测量View的宽高。（测量的宽高并不一定等于View的最终宽高）
- 对于顶级View(DecorView)其由窗口的尺寸和其自身的LayoutParams来共同确定。
- 对于普通的View,其MeasureSpec由父容器的MeasureSpec和自身的LayoutParams来共同决定。MeasureSpec一旦确定以后，onMeasure中就可以确定View的测量宽高。
- 测量模式：
	UNSPECIFIED:父容器不对View有任何的限制,要多大就给多大,这种情况一般用于系统内部，表示一种测量的状态。

	EXACTLY:父容器已经检测出View所需要的精确大小，这个时候的View的最大的值就是SpecSize的所指定的值，对应于match_parent和一些具体的数值。

	AT_MOST:父容器指定一个可用大小即SpecSize,View的大小不能大于这个值,对应于View的wrap_content.

- 对于普通的View,其MeasureSpec由父容器的MeasureSpec和自身的layoutParams来共同决定.