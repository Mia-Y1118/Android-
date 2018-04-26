# 一、View的事件分发机制： #

1、点击事件的分发过程由dispatchTouchEvent，onInterceptTouchEvent,onTouchEvent.
①dispatchTouchEvent:用来进行事件的分发，如果事件能够传递给当前View,那么此方法一定被调用，返回的结果收到当前View的onTouchEvent,以及下级的dispatchTouchEvent方法的影响，表示是否消费当前的事件。
②onInterceptTouchEvent:用来判断是否拦截某个事件，如果当前的View拦截了某个事件，那么在同一个事件序列当中，此方法不会被再次调用，返回的结果表示是否拦截当前的事件。
③onTouchEvent:在diapatchEvent方法中调用，用来处理点击事件，返回的结果表示是否消耗掉当前的事件。

2、事件的分发流程：
对于一个ViewGroup来说，点击事件产生后，首先会传递给它，它的diapatchTouchEvent就会被调用，如果这个ViewGroup的onInterceptTouchEvent的方法返回true，表示它要拦截当前的事件，接着事件就会交给这个ViewGroup来处理，即这个ViewGroup的onTouch方法就会被调用。如果这个ViewGroup的onInterceptTouchEvent返回为false,表示它不拦截当前的事件，这是当前的事件会继续传递给它的子元素。接着子元素的dispatchTouchEvent方法会被调用，直到当前的事件被消耗。

# 二、View的工作原理 #

1、ViewRoot和DecorView:
①ViewRoot对应于ViewRootImpl,是连接WindowManager和DecorView的纽带。View的绘制流程是从ViewRoot的PerformTraversals方法开始的，它经过Measure,layout,draw三个过程，最终才能将一个View 绘制出来，其中measure是用来测量View的宽和高，layout是用来确定View在父容器中的放置位置，确定View的自重宽/高和四个顶点的位置，而draw则负责将View绘制在屏幕上。
②DecorView做为顶级View,是一个FrameLayout,其包含两部分，其中一部分为Title,另外一部分为content,在Activity中设置this.setContent时则是设置content的布局。

2、MeasureSpec:其很大程度上决定了一个View的规格尺寸（在测量过程中，系统会将View的LayoutParams根据父容器所施加的规则换成对应的MeasureSpec,然后再根据这个measureSpec来测量View的宽和高。

①MeasureSpec:代表一个32位的int值，高两位代表SpecMode(UNSPECIFIED：父容器不对View有任何的限制，要多大就给多大,EXACTLY：父容器已经测量出View的精确大小，这个时候View 的最终大小就是SpecSize所指定的值,AT_MOST：父容器指定了一个可用大小SpecSize,View的大小不能大于这个值),低30位代表SpecSize,是指某种测量模式下的规格大小。
②LayoutParams需要和父容器一起才能确定View的MeasureSpec,从而进一步决定View的宽和高。
③LayoutParams.MATCH_PARENT:精确模式，大小就是窗口的大小。
LayoutParams.WRAP_CONTENT:最大模式，大小不定，但是不能超过窗口的大小
固定大小：精确模式，大小为LayoutParams中指定的大小。

# 三、View的工作流程： #
## 1、measure过程： ##
如果是一个原始的View,那么通过measure方法就可以完成其测量过程。如果是一个ViewGroup,除了完成自己的测量过程外，还会便利去调用整个子元素的measure方法。

- ①View的measure过程：在View的measure方法中，会去调用View的onMeasure，并且直接继承View的自定义控件需要重写onMeasure方法并设置wrap_content时的自身大小，否则在布局中使用wrap_content就相当于使用了match_parent.
 
- ②ViewGroup的measure过程：没有重写View的onMeasure方法，但是其提供了一个**measureChildren**的方法，其思想就是取出子元素的LayoutParams,然后再通过getChildMeasureSpec来创建子元素的MeasureSpec,接着直接传递给View的Measure方法来测量。
 
- ③在Activity的onCreate,onStart,onResume中均无法正确得到某个View的宽/高，这是因为View的measure过程和Activity的生命周期不是同步执行的，因此无法保证Activity执行了onCreate，onStart,onResume时某个View已经测量完毕了，如果View还没有测量完毕，那么获得的宽/高就是0.
 
	解决方法是在onWindowFocusChanged的方法去获取View的高/宽。

## 2、layout的过程： ##
- ①大致流程：首先会通过setFrame的方法来设定View的四个顶点的位置，即初始化mLeft,mRight,mTop,mBottom.View的四个顶点一旦确定，那么View在父容器的位置也就确定了；接着会调用onLayout方法，这个方法的用途是父容器确定子元素的位置

- ②在View的默认的实现中，View的测量宽/高和最终的宽/高是相等的，只不过测量的宽/高形成与View的measure过程，而最终的宽/高形成于View的layout过程，即两者赋值的时间不一致。
 
- ③如果重写了layout,则测量的宽高和最终的宽高有可能不同。

## 3.draw的过程：绘制过程的传递是通过dispatchDraw来实现的： ##

①View的绘制过程遵循如下几步：

- a.绘制背景Background.draw(canvas)
- b.绘制自己（onDraw)
- c.绘制children(dispatchDraw)
- d.绘制装饰（onDrawScrollBars)

4、自定义View:大概分为4类，①继承View,重写onDraw的方法 ②继承ViewGroup派生特殊的Layout ③继承特定的View(TextView)	④继承特定的ViewGroup(LinearLayout)

5、自定义View的注意事项：
①让View支持wrap_content
②让View支持Padding:如果不在draw方法中处理Padding,那么Padding属性无法起作用，另外直接继承自ViewGroup的空间需要在onMeasure和onLayout中考虑padding,margin对其造成的影响，不然将导致padding和margin失效。
③如果View中有线程或者动画，当View变得不可见的时候，需要及时停止，否则有可能造成内存泄漏。