### 一、View的基础知识:

##### 1、View本身就可以是单个控件,也可以是多个控件组成的一组控件。		

	- View的位置主要由它的四个顶点来决定，分别对应于View的四个属性，Top(左上角纵坐标）,left（左上角横坐标）,right（右下角横坐标）,bottom（右下角纵坐标）。
	- 坐标都是相对于View的容器。（View在平移的过程中，top,left表示的原始左上角的位置信息的值并不会发生改变,改变的是TranslationX,TranslationY。 x = left + TranslationX ,y = right + TranslationY)

##### 2、View的一些参数：

 -  MotionEvent : 
    - ACTION_DOWN
    - ACTION_MOVE
    - ACTION_UP
    - getX/getY返回的是View相对于父容器的左上角坐标，getRawX/getRawY返回的是相对于手机屏幕左上角的坐标。

 - TouchSlop:是系统所能识别的最小的可认为是滑动的距离

   - ViewConfiguration.get(getContext()).getScaledTouchSlop()来获取这个常量,如果两次滑动的距离小于这个值,系统就不认为是滑动操作。

     

 - VelocityTracker:用于追踪手在滑动过程中的速度,包括水平和竖直方向的速度

   - 获取速度之前必须计算速度,再获取垂直和水平方向的速度,当不需要使用它的时候,需要调用clear来重置并回收内存。

     ```kotlin
     val velocityTracker = VelocityTracker.obtain()
     velocityTracker.addMovement(event)
     velocityTracker.computeCurrentVelocity(1000)
     val xVelocity = velocityTracker.getXVelocity()
     val yVelocity = velocityTracker.getYVelocity()
     
     //用完之后
     velocityTracker.clear()
     velocityTracker.recycle()
     ```

 - GestureDetector:用于辅助检测用户的单击，滑动，长按，双击等行为(建议：如果只是滑动监听，则可在onTouchEvent中实现,如果要监听双击行为，则使用GestureDetector：)

   ```kotlin
   val mGestureDetector = GestureDetector(this)
   //解决长按屏幕后无法拖动的问题
   mGestureDetector.setIsLongpressEnabled(false)
   
    override fun onTouchEvent(event: MotionEvent?): Boolean {
           val consume = mGestureDetector.onTouchEvent(event)
           return consume
   }
   ```

 - Scroller:View的scrollTo/scrollBy的滑动过程都是瞬间完成的，没有过渡效果。可以使用Scroller来实现有过度效果的滑动。

   - 实现原理：Scroller本身并不能实现View的滑动，需要配合computeScroll方法才能完成弹性滑动的效果，它不断让View重绘，而每一次重绘距滑动起始时间会有一个时间间隔，再调用scrollTo，每次滑动小幅度，重复重绘，就组成了弹性滑动。
   
   ```kotlin
   val scroller = Scroller(context)
   private fun smoothScrollTo(destX : Int,destY : Int){
     val scrollX = getScrollX()
     val delta = destX - scrollY
     mScroller.startScroll(scroller,0,delta,0,1000)
     invalidate()
   }
   
   override fun computeScroll(){
     if(mScroller.computeScrollOffset()){
       scrollTo(mScroller.getCurrX(),mScroller.getCurrY())
       postInvalidate()
     }
   }
   ```

##### 3、常见的滑动方式：

- 通过View本身提供的scrollTo/scrollBy

  - 实现滑动效果，但是并不影响内部元素的点击事件,但是只能滑动View的内容，不能滑动View本身

- 通过动画(View 动画，属性动画）给View施加平移效果来实现滑动。

  - View动画 ： 滑动的是View的影像,当点击事件的时候还是原位置有效(适用于无交互的View)
  - 属性动画：3.0以上使用，实现滑动后点击事件依然生效

- 通过改变View的LayoutParams使得View重新布局来实现滑动，操作稍微复杂,适用于有交互的View。

  - 先获取组件的参数

  - 对params做改变，重新设置一些值

  - 组件重现设置参数

  ```java
  MarginLayoutParams params = (MaginLayoutParams)mButton1.getLayoutParams();
  params.width += 100;
  params.leftMargin += 100;
  mButton1.requestLayout();
  //或者 mButton1.setLayoutParams(params);
  ```

  




