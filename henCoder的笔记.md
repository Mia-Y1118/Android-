##自定义View
- Paint:画笔
	Paint.setStyle(Style style)设置绘制模式
	Paint.setColor(int color)设置颜色
	Paint.setStrokeWidth(float width)设置线条宽度
	Paint.setTextSize(float textSize)设置文字大小
	Paint.setAntiAlias(boolean aa)设置抗锯齿开关

##颜色填充
- drawColor(Color.BLACK):会把整个区域染成纯黑色，覆盖掉原来的内容
- drawColor(Color.parse("#88880000") 会在原有的绘制效果上加一层半透明的红色遮罩。
- drawRGB(int r,int g,int b)
- drawARGB(int a,int r,int g,int b)

##画圆
- drawCircle(float centerX, float centerY, float radius, Paint paint) 画圆

- 前两个参数 centerX centerY 是圆心的坐标，第三个参数 radius 是圆的半径，单位都是像素，它们共同构成了这个圆的基本信息（即用这几个信息可以构建出一个确定的圆）；第四个参数 paint 我在视频里面已经说过了，它提供基本信息之外的所有风格信息，例如颜色、线条粗细、阴影等。

- paint.setColor(Color.RED); // 设置为红色  
  canvas.drawCircle(300, 300, 200, paint);  

-paint.setStyle(Paint.Style.STROKE); // Style 修改为画线模式  
 canvas.drawCircle(300, 300, 200, paint);  
 setStyle(Style style) 这个方法设置的是绘制的 Style 。Style 具体来说有三种： FILL, STROKE 和  FILL_AND_STROKE 。FILL 是填充模式，STROKE 是画线模式（即勾边模式），FILL_AND_STROKE 是两种模式一并使用：既画线又填充。它的默认值是 FILL，填充模式。

- Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG);  //抗锯齿
- drawPath(Path path, Paint paint) 画自定义图形


##Paint的着色：
- Paint设置颜色的方法有2种，一种是直接用Paint.setColor/ARGB的方式来设置颜色，另外一种是通过Share来指定着色方案。

- Shader:着色器，用于绘制颜色。当Paint在绘制图形和文字时就不使用setColor/ARGB（）设置的颜色了，而是使用Shader的方案来上颜色。并且在Android的绘制种不是直接使用Shader,而是使用它的几个子类（LinearGradient,RadialGradient,SweepGradient,BitmapShader,ComposeShader)

- 在构造着色器的时候，最后一个参数是着色规则TileMode，一共有3个值，CLAMP，MIRROR:镜像，REPEAT：重复.

- 线条宽度 0 和 1 的区别：默认情况下，线条宽度为 0，但你会发现，这个时候它依然能够画出线，线条的宽度为 1 像素。那么它和线条宽度为 1 有什么区别呢？其实这个和后面要讲的一个「几何变换」有关：你可以为 Canvas 设置 Matrix 来实现几何变换（如放大、缩小、平移、旋转），在几何变换之后 Canvas 绘制的内容就会发生相应变化，包括线条也会加粗，例如 2 像素宽度的线条在 Canvas 放大 2 倍后会被以 4 像素宽度来绘制。而当线条宽度被设置为 0 时，它的宽度就被固定为 1 像素，就算 Canvas 通过几何变换被放大，它也依然会被以 1 像素宽度来绘制。Google 在文档中把线条宽度为 0 时称作「hairline mode（发际线模式）」。

- setStrokeCap(Paint.Cap cap)设置线头的形状，线头的形状主要有3种，BUTT平头，ROUND圆头，SQUARE方头，默认为BUTT。

-setStrokeJoin(Paint.Join join)设置拐角的形状：可以有3个值，MITER尖角，BEVEL平角，ROUND圆角，默认为MITER。

##色彩优化：
- setDither(boolean dither):设置图像的抖动效果，目前抖动效果没有那么实用了，如果选择的是16位的，开启比较明显。
- setFilterBitmap(boolean filter):是否使用双线过滤来绘制Bitmap
- setPathEffect(PathEffect effect):给图形设置轮廓效果。

##Canvas绘制文字

- Canvas的文字绘制的方法有3个，drawText(),drawTextRun(),drawTextOnPath().

- drawText(String text,float x,float y,Paint paint):x,y是文字左下角的位置坐标,y指的是基线。

- drawTextRun()是加入了两项额外的设置，上下文和文字方向。

- drawTextOnPath():使用的Path,拐弯的地方应该用圆角，别用尖角。drawTextOnPath(String text, Path path, float hOffset, float vOffset, Paint paint)： hOffset 和 vOffset。它们是文字相对于 Path 的水平偏移量和竖直偏移量，利用它们可以调整文字的位置。例如你设置 hOffset 为 5， vOffset 为 10，文字就会右移 5 像素和下移 10 像素。

-StaticLayout:支持换行，不是一个View,也不是一个ViewGroup。是纯粹用来绘制文字的。

	String text1 = "Lorem Ipsum is simply dummy text of the printing and typesetting industry.";  
	StaticLayout staticLayout1 = new StaticLayout(text1, paint, 600, Layout.Alignment.ALIGN_NORMAL, 1, 0, true);
	canvas.save();  
	canvas.translate(50, 100);  
	staticLayout1.draw(canvas); 
	canvas.restore();  

	width 是文字区域的宽度，文字到达这个宽度后就会自动换行； 
	align 是文字的对齐方向； 
	spacingmult 是行间距的倍数，通常情况下填 1 就好； 
	spacingadd 是行间距的额外增加值，通常情况下填 0 就好； 
	includeadd 是指是否在文字上下添加额外的空间，来避免某些过高的字符的绘制出现越界。

- setFakeBoldText(boolean fakeBoldText)为是否显示粗体
- setStrikeThruText(boolean strikeThruText)是否添加删除线
- setUnderlineText(boolean underlineText)是否添加下划线
- setTextSkewX(float skewX)设置文字横向错切角度，其实就是文字倾斜度
- setTextScaleX(float scaleX):设置文字横向缩放，也就是文字的变胖变瘦问题
- setLetterSpacing(float letterSpacing)设置字符的间距
- setTextAlign(Paint.Align align)用来设置文字的对齐方式
- sethinting(int mode):微调字体

#对于View的变换:
##裁剪的使用ClipRect()和ClipPath()
- clipRect:

		canvas.save();
		canvas.clipRect(left, top, right, bottom);  
		canvas.drawBitmap(bitmap, x, y, paint);  
		canvas.restore();

-clipPath():将Rect替换成Path

##几何变换：
- 几何变换的使用大概分为三类：

		1、使用Canvas来做常见的二维变换，需要先调用canvas.save(),做完变换之后canvas.restore();
		2、使用Matrix来做常见和不常见的二维变换
		3、使用Camera来做三维变换

- Canvas来做变换：

		1、使用Canvas.translate(float dx,float dy)平移
		2、使用Canvas.rotate(float degrees,float px,float py)旋转：角度，方向是顺时针方向，px和py是轴心的位置
		3、Canvas,scale(float sx,float sy,float py)放缩：sx和sy是横向和纵向的放缩倍数，px,py是放缩的轴心。
		4、错切：Canvas.skew(float sx,float sy):sx是x方向的错切系数，sy是y方向的错切系数

- 使用Matrix来做变换：

		1、创建Matrix对象
		2、调用Matrix的Pre/postTranslate/Rotate/Scale/Skew()方法来设置几何变换
		3、使用Canvas.setMatrix(matrix)或者Canvas.concat(matrix)来做几何变换用用到Canvas.
	
#####把 Matrix 应用到 Canvas 有两个方法： Canvas.setMatrix(matrix) 和 Canvas.concat(matrix)。
		
		4、Canvas.setMatrix(matrix)：用 Matrix 直接替换 Canvas 当前的变换矩阵，即抛弃 Canvas 当前的变换，改用 Matrix 的变换（注：根据下面评论里以及我在微信公众号中收到的反馈，不同的系统中  setMatrix(matrix) 的行为可能不一致，所以还是尽量用 concat(matrix) 吧）；

		5、Canvas.concat(matrix)：用 Canvas 当前的变换矩阵和 Matrix 相乘，即基于 Canvas 当前的变换，叠加上 Matrix 中的变换。

- 使用Camera来做三维变换（Camera的三维变换有三类：旋转，平移，移动相机）
		1、Camera.rotate*():一共有四个方法：rotateX(deg),rotateY(deg),rotateZ(deg),rotate(x,y,z)
			
				canvas.save();
				
				camera.save(); // 保存 Camera 的状态  
				camera.rotateX(30); // 旋转 Camera 的三维空间  
				camera.applyToCanvas(canvas); // 把旋转投影到 Canvas  
				camera.restore(); // 恢复 Camera 的状态
				
				canvas.drawBitmap(bitmap, point1.x, point1.y, paint);  
				canvas.restore();  

		2、Camera.setLocation(x, y, z) 设置虚拟相机的位置 ：注意！这个方法有点奇葩，它的参数的单位不是像素，而是 inch，英寸。

##属性动画：

- 动画分为两类：（Animation和Transition),其中Animation分为ViewAnimation和Property Animation两类，
 
		ViewAnimation是纯粹的基于Framework的绘制转变。
		PropertyAnimation:是属性动画，是Android3.0开始引入的新的动画形式。
		Transition:是在Android中切换界面的动画的效果，其重点在于切换而不是动画。

-ViewPropertyAnimator:

		1、View.animate().translationX(500);

- View 的每个方法都对应了 ViewPropertyAnimator 的两个方法，其中一个是带有 -By 后缀的，例如，View.setTranslationX() 对应了 ViewPropertyAnimator.translationX() 和  ViewPropertyAnimator.translationXBy() 这两个方法。其中带有 -By() 后缀的是增量版本的方法，例如，translationX(100) 表示用动画把 View 的 translationX 值渐变为 100，而 translationXBy(100) 则表示用动画把 View 的 translationX 值渐变地增加 100。

- ObjectAnimator:

	1、如果是自定义控件，需要添加 setter / getter 方法；
	2、用 ObjectAnimator.ofXXX() 创建 ObjectAnimator 对象；
	3、用 start() 方法执行动画。

- 属性动画的通用功能：
	
		1、setDuration(int duration):设置动画时长
		
		// imageView1: 500 毫秒
		imageView1.animate()  
        .translationX(500)
        .setDuration(500);

		// imageView2: 2 秒
		ObjectAnimator animator = ObjectAnimator.ofFloat(  
		        imageView2, "translationX", 500);
		animator.setDuration(2000);  
		animator.start();  

		2、setInterpolator(Interpolator interpolator)设置Interpolator:速度设置器
		new AccelerateDecelerateInterpolator():先加速再减速，不设置的时候默认的加速器
		new LinearInterpolator()线性匀速的
		new AnticipateOvershootInterpolator():带施法前摇和回弹的Interpolator.
		new AccelerateInterpolator():持续加速
		new DecelerateInterpolator():持续减速直到0
		new AnticipateInterpolator():先拉一下进行正常动画轨迹，再回到原来的位置
		new OvershootInterpolator():动画超过目标值再弹回一点
		......
		还可以进行自定义path的Interpolator();

-给动画设置监听：
	
	ViewPropertyAnimator用的是setListener()和setUpdateListener()方法，可以设置一个监听器，要移除监听器时通过setListener(null),setUpdateListener(null)来移除监听

	ObjectAnimator:用的是addListener()和addUpdateListener()来添加一个或者多个监听器，移除监听则是通过removeListener()或者removeUpdateListener()来指定移除的对象


	


##Property Animation属性动画进阶
- 针对特殊类型的属性来做属性动画
	关于ObjectAnimator,ke
	
	TypeEvalutor: