# 一、View的基础知识 #
## 1、View的基本概念
- View是所有组件的基类，View本身可以是单个控件也可以是由多个控件组成的一组控件
![](https://i.imgur.com/tw276pY.png)
- 坐标系:

	Android屏幕坐标系：

	![](https://i.imgur.com/1VpTMli.png)

	**View坐标系**

	![](https://i.imgur.com/GkfnnfK.png)

		getTop：当前View相对于其父View的坐标（View提供的方法)
		getY：当前所触摸的点，相对于本身View的坐标（MotionEvent提供的方法)
		getRowY:当前Viewd的所触摸的点，相对于整个手机屏幕的坐标(MotionEvent提供的方法）
	![](https://i.imgur.com/iTRHq3e.png)


# 二、View的工作原理
## 1、ViewRoot
- View与手机屏幕的关系

	![](https://i.imgur.com/sLvaNNn.png)

- ViewRoot对应于ViewRootImpl类,是连接WindowManager和DecorView的纽带，View的绘制流程是从ViewRoot的PerformTraversals方法开始的，它经过Measure,layout,draw三个过程，最终才能将一个View 绘制出来，其中:

		measure是用来测量View的宽和高
		layout是用来确定View在父容器中的放置位置，确定View的自重宽/高和四个顶点的位置
		draw负责将View绘制在屏幕上。

## 2、DecorView
- DecorView做为顶级View,是一个FrameLayout,其包含两部分，其中一部分为Title,另外一部分为content,在Activity中设置this.setContent时则是设置content的布局。

## 3、MeasureSpec:很大程度上决定了一个View的规格尺寸（在测量过程中，系统会将View的LayoutParams根据父容器所施加的规则换成对应的MeasureSpec,然后再根据这个measureSpec来测量View的宽和高。
- MeasureSpec:代表一个32位的int值，高两位代表SpecMode，低30位代表SpecSize,是指某种测量模式下的规格大小。

	UNSPECIFIED：父容器不对View有任何的限制，要多大就给多大

	EXACTLY：父容器已经测量出View的精确大小，这个时候View 的最终大小就是SpecSize所指定的值

	AT_MOST：父容器指定了一个可用大小SpecSize,View的大小不能大于这个值

## 4、LayoutParams：需要和父容器一起才能确定View的MeasureSpec,从而进一步决定View的宽和高。
- LayoutParams.MATCH_PARENT:精确模式，大小就是窗口的大小。
- LayoutParams.WRAP_CONTENT:最大模式，大小不定，但是不能超过窗口的大小
- 固定大小：精确模式，大小为LayoutParams中指定的大小。

# 三、View的工作流程： #
## 1、measure过程
### 如果是一个原始的View,通过measure方法就可以完成其测量过程。如果是一个ViewGroup,除了完成自己的测量过程外，还会便利去调用整个子元素的measure方法。

- ①View的measure过程：在View的measure方法中，会去调用View的onMeasure，并且直接继承View的自定义控件需要重写onMeasure方法并设置wrap_content时的自身大小，否则在布局中使用wrap_content就相当于使用了match_parent.
 
- ②ViewGroup的measure过程：没有重写View的onMeasure方法，但是其提供了一个**measureChildren**的方法，其思想就是取出子元素的LayoutParams,然后再通过getChildMeasureSpec来创建子元素的MeasureSpec,接着直接传递给View的Measure方法来测量。
 
- ③在Activity的onCreate,onStart,onResume中均无法正确得到某个View的宽/高，这是因为View的measure过程和Activity的生命周期不是同步执行的，因此无法保证Activity执行了onCreate，onStart,onResume时某个View已经测量完毕了，如果View还没有测量完毕，那么获得的宽/高就是0.
 
	解决方法是在onWindowFocusChanged的方法去获取View的高/宽。

## 2、layout的过程
- ①大致流程：首先会通过setFrame的方法来设定View的四个顶点的位置，即初始化mLeft,mRight,mTop,mBottom.View的四个顶点一旦确定，那么View在父容器的位置也就确定了；接着会调用onLayout方法，这个方法的用途是父容器确定子元素的位置

- ②在View的默认的实现中，View的测量宽/高和最终的宽/高是相等的，只不过测量的宽/高形成与View的measure过程，而最终的宽/高形成于View的layout过程，即两者赋值的时间不一致。
 
- ③如果重写了layout,则测量的宽高和最终的宽高有可能不同。

## 3.draw的过程：绘制过程的传递是通过dispatchDraw来实现的

①View的绘制过程遵循如下几步：

- a.绘制背景Background.draw(canvas)
- b.绘制自己（onDraw)
- c.绘制children(dispatchDraw)
- d.绘制装饰（onDrawScrollBars)


# 四、自定义View的注意事项

- ①让View支持wrap_content
- ②让View支持Padding:如果不在draw方法中处理Padding,那么Padding属性无法起作用，另外直接继承自ViewGroup的空间需要在onMeasure和onLayout中考虑padding,margin对其造成的影响，不然将导致padding和margin失效。
- ③如果View中有线程或者动画，当View变得不可见的时候，需要及时停止，否则有可能造成内存泄漏。