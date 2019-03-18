一、Android的动画的分类：（实现方式有：xml资源文件方式，代码方式）

```java
逐帧动画（Drawable Animation)：即顺序播放事件做好的图像，跟电影类似。

补间动画：（Tween动画)Android最早的动画框架，通过对场景里的对象不断做图像变换（平移，缩放，旋转）产生动画效果。

属性动画：补间动画增强版，支持对对象执行动画（很强大）。

过渡动画：实现Activity或者View间的过度动画效果。
```

二、Android逐帧动画：传一组图片进去，然后依次循环播放，可以设置每一张图片的播放时间，但是动画不连续。

```java
xml资源文件方式：（android:oneshot用来控制是否循环播放，true代表不会循环播放，false表示会循环播放。android:duration="200"表示每一帧持续播放的时间)
    <?xml version="1.0" encoding="utf-8"?>
    <animation-list xmlns:android="http://schemas.android.com/apk/res/android"
                    android:oneshot="false">
        <item android:drawable="@mipmap/run_1" android:duration="150" />
        <item android:drawable="@mipmap/run_2" android:duration="150" />
        <item android:drawable="@mipmap/run_3" android:duration="150" />
        <item android:drawable="@mipmap/run_4" android:duration="150" />
    </animation-list>

    ImageView image = (ImageView) findViewById(R.id.iv_1);
    image.setImageResource(R.drawable.animation_list);
    AnimationDrawable anim = (AnimationDrawable) image.getDrawable();
    anim.start();
```

三、Android补间动画（Tween动画）：不需要关心每一帧，只需要定义动画开始和结束两个关键帧，并指定动画变化的时间与方式等。但是不支持自定义View的拓展，其致命的一个缺点是控件的属性并没有发生改变。比如一个Button将一个地方移动到另一个地方，点击事件还是在原来的地方。

```java
透明度变化      alph      AlphaAnimation :渐变透明度动画效果
大小缩放变化    scale     ScaleAnimation:渐变尺寸伸缩动画效果
位置变换        translate   TranslateAnimation   画面转换位置移动动画效果
旋转变换        rotate    RotateAnimation    画面转移旋转动画效果
插值器（Interpolator：时间插值器，定义东环变换的速度)：Android系统会在补间动画开始和结束关键帧之间插入渐变值，它依据差值器。

举例：xml方式：
        <?xml version="1.0" encoding="utf-8"?>
    	<set xmlns:android="http://schemas.android.com/apk/res/android"
        		   android:shareInterpolator="true">
        <rotate android:fromDegrees="0"
                android:toDegrees="359"
                android:duration="1000"
                android:repeatCount="1"
                android:repeatMode="reverse"
                android:pivotX="50%"
                android:pivotY="50%"/>
    	</set>
在代码中使用：
    	LinearLayout layout = (LinearLayout)findViewById(R.id.layout_1);
		Animation animation = AnimationUtils.loadAnimation(this, R.anim.tween_1);
		layout.startAnimation(animation);

代码方式：
    AlphaAnimation,RotateAnimation,ScaleAnimation,TranslateAnimation...
    AlphaAnimation anim = new AlphaAnimation(1.0f,0.5f);
    anim.setDuration(2000);     // 动画单次播放时长为2秒
    anim.setRepeatCount(2);		// 动画播放次数
    anim.setRepeatMode(Animation.REVERSE); // 动画播放模式为REVERSE
    anim.setFillAfter(true);    // 设定动画播放结束后保持播放之后的效果
    iv.startAnimation(anim);     // 开始播放，iv_anim是一个ImageView控件
```

四、Android属性动画：（Animator):补间动画的增强版，由于补间动画只能够在视图View上，无法对非View的对象进行动画的操作。并且其没有改变View的属性，只是改变了视觉效果。

```java
属性动画的特点：任意Java对象，不再局限于视图View对象，实现的动画效果不局限于4种基本变换（平移，旋转，缩放，透明度）

工作原理：在一定的时间间隔内，不断对值进行改变，并不断将值赋给对象的属性，从而实现该对象在该属性上的动画效果。(底层的实现原理是一个数值发生器，和控件没有关系)

属性动画基类：Animator,抽象类。子类有两个重要的类：ValueAnimator和ObjectAnimator,Evaluator,AnimatorSet.单一动画实现的效果相当有限，更多的场景是同时适用多种动画效果，即组合动画。

ValueAnimator:数值发生器，是属性动画的根本，其功能就是在两个数值范围内，顺序地产生过渡数值，过渡速率可以通过Intepolator来控制，过渡时间通过duration来设置，最终产生一组有特定变化速率的数值序列。也可以用在自定义的View.
代码方式：
    ValueAnimator animator = ValueAnimator.ofFloat(0, 360);
    animator.setDuration(1000);
    animator.setInterpolator(new AccelerateInterpolator());
    animator.setRepeatCount(1);
    animator.setRepeatMode(ValueAnimator.REVERSE);
    animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
        @Override
        public void onAnimationUpdate(ValueAnimator valueAnimator) {
            float value = (float)valueAnimator.getAnimatedValue();
            imageView.setRotationY(rotate);
        }
     });
    animator.start();


ObjectAnimator:将ValueAnimator封装在里面，简单地实现对控件的动画。第一行代码有三个参数，分别是做动画的控件，动画值的属性名称，后面两个表示动画值的范围。
代码方式：
    ObjectAnimator animator = ObjectAnimator.ofFloat(imageView, "rotationY", 0, 359);
    animator.setDuration(1000);
    animator.setRepeatCount(1);
    animator.setRepeatMode(ValueAnimator.REVERSE);
    animator.start();

ObjectAnimator:还可以根据Path路径做动画.第一个是做动画的控件，第二和第三个是动画的方向。第四个是动画的Path路径。
    ObjectAnimator animator = ObjectAnimator.ofFloat(iv, View.X, View.Y, path);
    animator.setDuration(2000);
    animator.setRepeatCount(1);
    animator.setRepeatMode(ValueAnimator.REVERSE);
    animator.start();

    //注意：终点位置减去起点位置如果为正数，则是向下的属性动画。如果为负数则是向上的属性动画。
    Animator animator = ObjectAnimator.ofFloat(iv_contribute, "translationY", 0, -7);
    animator.setDuration(500);
    animator.start();
     
    
    
Interpolator(插值器)：用来控制动画过程的变化速率，没有他的动画将会一直匀速变化。
[Linear]（匀速）、[Accelerate]（加速）、[Decelerate]（减速）、[AccelerateDecelerate]（先加速，后减速）、[Anticipate]、[Overshoot]、[Bounce]、[Cycle]
```

五、揭露动画：CircularReveral

```java
//五个参数分别是View，中心点坐标，开始半径，结束半径。通过这五个参数的配合，我们可以做出很多不同的效果。
Animator anim = ViewAnimationUtils.createCircularReveal(oval, oval.getWidth() / 2, oval.getHeight() / 2, oval.getWidth(), 0);
anim.setDuration(2000);
anim.start();
```

六、Activity的转场动画

```java
//1、传统的转场动画：只能作用于整个页面，不能对页面中的单个元素做控制
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate android:fromXDelta="100%"
               android:toXDelta="0%"
               android:duration="500" />
</set>

<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate android:fromXDelta="0%"
               android:toXDelta="-40%"
               android:duration="500" />
</set>

startActivity(new Intent(this, Activity2.class));
overridePendingTransition(R.anim.slide_right_in, R.anim.slide_left_out);

//2、5.0新转场动画：分为4种，Explode、Slide、Fade、Share。可以对页面的单个元素做控制

a、Explode:下一个页面的元素从四面八方进入，最终形成完整的页面
	// 跳转
Intent intent = new Intent(this, CActivity.class);
startActivity(intent, ActivityOptions.makeSceneTransitionAnimation(this).toBundle());
	// 跳转的Activity   getWindow().setEnterTransition(new Explode());在setContent之前
public class CActivity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        getWindow().setEnterTransition(new Explode());
        setContentView(R.layout.activity_c);
    }
}

b、Slide:下一个页面元素从底部一次向上运动，最终形成完整的页面
	// 跳转
Intent intent = new Intent(this, CActivity.class);
startActivity(intent, ActivityOptions.makeSceneTransitionAnimation(this).toBundle());
	// 跳转的Activity
public class CActivity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        getWindow().setEnterTransition(new Slide());
        setContentView(R.layout.activity_c);
    }
}

c、Fade:Fade就是下一个页面元素渐变出现，最终形成完整的页面
	// 跳转
Intent intent = new Intent(this, CActivity.class);
startActivity(intent, ActivityOptions.makeSceneTransitionAnimation(this).toBundle());
	// 跳转的Activity
public class CActivity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        getWindow().setEnterTransition(new Fade());
        getWindow().setExitTransition(new Fade());
        setContentView(R.layout.activity_c);
    }
}

d、Share最复杂的一种转场，在跳转的两个Activity之间，如果有相同的View元素，则形成共享状态。
<!-- 首先，两个Activity共享的元素需要设置相同的transitionName： android:transitionName="fab" -->
<Button
     android:id="@+id/fab_button"
     android:layout_width="56dp"
     android:layout_height="56dp"
     android:background="@mipmap/ic_launcher"
     android:elevation="5dp"
     android:onClick="explode"
     android:transitionName="fab" />

<Button
     android:id="@+id/fab_button"
     android:layout_width="160dp"
     android:layout_height="160dp"
     android:layout_alignParentEnd="true"
     android:layout_below="@id/holder_view"
     android:layout_marginTop="-80dp"
     android:background="@mipmap/ic_launcher"
     android:elevation="5dp"
     android:transitionName="fab" />
// 跳转时，要为每一个共享的view设置对应的transitionName
View fab = findViewById(R.id.fab_button);
View txName = findViewById(R.id.tx_user_name);
intent = new Intent(this, CActivity.class);
startActivity(intent, ActivityOptions.makeSceneTransitionAnimation(this,
        Pair.create(view, "share"),
        Pair.create(fab, "fab"),
        Pair.create(txName, "user_name"))
        .toBundle());

// 跳转的Activity在onCreate方法中开启Transition模式
public class CActivity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        getWindow().requestFeature(Window.FEATURE_CONTENT_TRANSITIONS);
        setContentView(R.layout.activity_c);
    }
}
```