#一、fresco的默认占位图的设置：
两种方式：

- 在布局文件中设置：	<com.facebook.drawee.view.SimpleDraweeView  			xmlns:android="http://schemas.android.com/apk/res/android"	
	    **xmlns:app="http://schemas.android.com/apk/res-auto"**
	    android:layout_width="match_parent"
	    android:layout_height="130dp"
	    android:layout_marginLeft="15dp"
	    android:layout_marginRight="15dp"
	    android:layout_marginTop="12dp"
	    **app:placeholderImage="@drawable/free_course_bg"**
	    android:id="@+id/iv_free_course">
	</com.facebook.drawee.view.SimpleDraweeView>

- 在代码中设置：
 
		GenericDraweeHierarchy hierarchy = mSimpleDraweeView.getHierarchy();
		hierarchy.setPlaceholderImage(R.drawable.placeholderId);
- 可以用建造者模式来设置：
 
		List<Drawable> backgroundsList;
		List<Drawable> overlaysList;
		GenericDraweeHierarchyBuilder builder =
    	new GenericDraweeHierarchyBuilder(getResources());
		GenericDraweeHierarchy hierarchy = builder
    			.setFadeDuration(300)
    			.setPlaceholderImage(new MyCustomDrawable())
    			.setBackgrounds(backgroundList)
    			.setOverlays(overlaysList)
   				.build();
		mSimpleDraweeView.setHierarchy(hierarchy);

#二、popupWindow的注意事项：
- popupWindow的setOutsideTouchable(true)是指设置外部可点。
- setFocusable(true)是指设置当前的popupWindow获取焦点，当点击外部的组件的时候，外部组件的单击事件不会生效，因为焦点没在外部组件上。（点击外部组件的时候该popupWindow消失，再点击一下，该组件的点击事件才会生效）。

#三、对于Json的解析：
- 在代码中观察接口返回的数据，如果返回具有rs,redesp,resultMessage则需要用项目中的封装的JSONObjectCallback()，其将返回的原生json已经经过了一层解析，当得到的response就直接是resultMessage.

- 有时候需要对返回的原生的json做解析，比如需要根据rs标志位来做一些处理的时候，就需要自己去解析Json.在项目中就用JSONObjectCallback2().
