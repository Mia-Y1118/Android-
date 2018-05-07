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
