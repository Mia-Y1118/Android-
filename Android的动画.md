一、Android的动画的分类：（实现方式有：xml资源文件方式，代码方式）

    逐帧动画（Drawable Animation)：即顺序播放事件做好的图像，跟电影类似。

    补间动画：通过对场景里的对象不断做图像变换（平移，缩放，旋转）产生动画效果。

    属性动画：补间动画增强版，支持对对象执行动画。

    过渡动画：实现Activity或者View间的过度动画效果。

二、Android逐帧动画：

    xml资源文件方式：（android:oneshot用来控制是否循环播放，true代表不会循环播放，false表示会循环播放。android:duration="200"表示每一帧持续播放的时间)


    逐帧动画的代码方式：


三、Android补间动画：不需要关心每一帧，只需要定义动画开始和结束两个关键帧，并指定动画变化的时间与方式等。
透明度变化   alph      AlphaAnimation :渐变透明度动画效果

大小缩放变化    scale   ScaleAnimation:渐变尺寸伸缩动画效果

位置变换      translate   TranslateAnimation   画面转换位置移动动画效果

旋转变换     rotate    RotateAnimation    画面转移旋转动画效果

插值器（Interpolator：时间插值器，定义东环变换的速度)：Android系统会在补间动画开始和结束关键帧之间插入渐变值，它依据差值器。

举例：xml方式：


代码方式：


    四、Android属性动画：（Animator)

补间动画的增强版，由于补间动画只能够在视图View上，无法对非View的对象进行动画的操作。并且其没有改变View的属性，只是改变了视觉效果。

属性动画的特点：任意Java对象，不再局限于视图View对象，实现的动画效果不局限于4种基本变换（平移，旋转，缩放，透明度）

工作原理：在一定的时间间隔内，不断对值进行改变，并不断将值赋给对象的属性，从而实现该对象在该属性上的动画效果。

属性动画基类：Animator,抽象类。子类有两个重要的类：ValueAnimator和ObjectAnimator,Evaluator,AnimatorSet.

单一动画实现的效果相当有限，更多的场景是同时适用多种动画效果，即组合动画。

xml方式：


    Java方式：


    递归绘制的方法：

1、绘制背景 

2、如果需要,保存画布(canvas),为淡入淡出做准备 

3、通过调用View.onDraw(canvas)绘制View本身的内容 

4、通过 dispatchDraw(canvas)绘制自己的孩子,dispatchDraw->drawChild->child.draw(canvas) 这样的调用过程被用来保证每个子 View 的 draw 函数都被调用 

5、如果需要，绘制淡入淡出相关的内容并恢复保存的画布所在的层（layer） 

6、绘制修饰的内容（例如滚动条） 