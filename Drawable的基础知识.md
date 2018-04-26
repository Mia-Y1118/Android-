#2018/03/23

#Android Drawable
##一、Drawable的分类：
###1、BitmapDrawable(可以直接使用图片，也可以通过xml文件来加载）
	①andorid：antialias 抗锯齿功能，如果开启会让图片变得平滑，但是在一定的程度上也降低了图片的清晰度，但是这个降低程度可以忽略，因此抗锯齿的功能应该开启。
	②android:dither 抖动效果，主要式当图片的像素配置和手机屏幕的像素配置不一致的时候，开启这个可以让高质量的低质量的屏幕上保持较好的显示效果。
	③android:filter 图像的过滤效果，一般当图片拉伸的时候可以保持良好的展示效果。
	④android:gravity:位置的显示
	⑤android:tileMode 平铺效果 disabled,clamp,repeat,mirror镜像等。
###2、ShapeDrawable通过corners,gradient,padding,size,solid,stroke等来实现效果。
	①可以通过颜色来构造的图形,既可以是一种纯色的图形，也可以实渐变效果的图形。
	<shape >
	</shape>
	②android:shape 有四个选项，rectangle,oval,line,ring.默认值是矩形。
	③android:corners 表示四个角的角度，仅仅针对于矩形。
	④<gradient>:渐变填充，与<solide>标签是排斥的，表示渐变效果。android:angle——0表示从左到右，90表示从下到上
	⑤<solid>表示纯色填充
	⑥<stroke>描边：

###3、LayerDrawable<layer-list>,表示一种层次化的drawable集合。
	①包含多个item:每一个item表示一个drawable,可以通过shape的形式来实现。
###4、StateListDrawable
	①<selector>,也表示一个drawable的集合,其中也是一个Item,主要用于一个组件的不同状态。
###5、自定义Drawable:其中主要的方法是drwable,setAlpha,setColorFilter,getOpacity
