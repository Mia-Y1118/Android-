#一、悬浮窗的实现
- 悬浮窗是当Activity切换的时候，悬浮窗不会消失
- 实现步骤是：

		①在Application中的监听Activity的生命周期的方法中加载一个View.
			 View view = LayoutInflater.from(activity).inflate(R.layout.view_float,null);
		②通过windowManager的形式去获取LayoutParams
 			 WindowManager.LayoutParams params = new WindowManager.LayoutParams();
       		 params.gravity = Gravity.BOTTOM | Gravity.RIGHT;

		③把上面的view添加到Activity的setContentView中。
        	 activity.addContentView(view,params);

		④在AndroidManifest中添加悬浮窗权限。
		 <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>

	