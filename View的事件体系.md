
#2018/03/16
##一、View的基础知识:
###1、View本身就可以是单个控件,也可以是多个控件组成的一组控件。		
	View的位置主要由它的四个顶点来决定，分别对应于View的四个属性，Top(左上角纵坐标）,left（左上角横坐标）,right（右下角横坐标）,bottom（右下角纵坐标）。这些坐标都是相对于View的容器。（View在平移的过程中，top,left表示的原始左上角的位置信息的值并不会发生改变,相当于是父容器平移，而当前View对于父容器的相对位置是不变的，此时发生改变的是x,y,translationX和translationY)

###2、View的一些参数：
	①getX/getY返回的是相对于当前View的左上角的x和y坐标，getRawX/getRawY返回的是相对于手机屏幕左上角的x和y的坐标。
	
	②TouchSlop:是系统所能识别的最小的可认为是滑动的距离。可以通过：ViewConfiguration.get(getContext()).getScaledTouchSlop()来获取这个常量,为8dp,如果两次滑动的距离小于这个值,系统就不认为是滑动操作。

	③VelocityTracker:用于追踪手在滑动过程中的速度,包括水平和竖直方向的速度。获取速度之前必须计算速度,再获取垂直和水平方向的速度,当不需要使用它的时候,需要调用clear来重置并回收内存。

	④GestureDetector:用于辅助检测用户的单击，滑动，长按，双击等行为。解决长按屏幕后无法拖动的问题,如果只是滑动监听，则可在onTouchEvent中实现,如果要监听双击行为，则使用GestureDetector：
	GestureDetector mGestureDetector = new GestureDetector(this)
	mGestureDetector.setIsLongpressEnabled(false)

	⑤Scroller:View的scrollTo/scrollBy的滑动过程都是瞬间完成的，没有过渡效果。
###3、常见的滑动方式：

- 通过View本身的提供的scrollTo/scrollBy,是view提供的原生的方法,它可以实现滑动的效果，但是并不影响内部元素的点击事件,但是只能滑动View的内容，不能滑动View本身。

- 通过动画给View施加平移效果来实现滑动,滑动的是View的影像,当点击事件的时候还是原位置有效,如果不需要响应用户的交互时，3.0以上的版本可以实现点击事件，动画是比较好的选择，主要使用与没有交互的View和实现复杂的动画效果。

- 通过改变View的LayoutParams使得View重新布局来实现滑动，操作稍微复杂,适用于有交互的View。
 
	①先获取组件的参数

	②对params做改变，重新设置一些值

	③组件重现设置参数


	








