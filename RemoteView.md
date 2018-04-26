#RemoteViews:
##一、RemoteView的基本概念：
###1、RemoteViews是一种远程View，可以在其他进程中显示,RemoteViews提供一组基础的操作用于跨进程更新它的UI。
###2、使用场景：通知栏,桌面小部件
###3、通知栏的实现：（Binder)
	主要通过Notification的形式，通过NotifyManager的notify的方法来实现。

	①系统默认：
	Notification notification = new Notification();
	notification.icon = R.drawable.ic_launcher;
	notification.tickerText = "hello world"
	notification.when = System.currentTimeMills();
	notification.flags = Notification.FLAG_AUTO_CANCEL;
	Intent intent = new Intent(this,DemoActivity.class);
	PendingIntent pendingIntent = PendingIntent.getActivity(this,0,intent,PendingIntent.FLAG_UPDATE_CURRENT);
	notification.setLastestEventInfo(this,"chaper_5","this is a noyification",pendingIntent);
	NotificationManager manager =(NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
	manager.notify(1,notification);
	
	②自定义的Notification:会使用到RemoteView,通过RemoteView来加载一个自定义的布局。

	Notification notification = new Notification();
	notification.icon = R.drawable.ic_launcher;
	notification.tickerText = "hello world"
	notification.when = System.currentTimeMills();
	notification.flags = Notification.FLAG_AUTO_CANCEL;
	Intent intent = new Intent(this,DemoActivity.class);
	PendingIntent pendingIntent = PendingIntent.getActivity(this,0,intent,PendingIntent.FLAG_UPDATE_CURRENT);
	RemoteViews remoteViews = new RemoteViews(getPackageName(),R.layout.layout_notigication);

	**remoteViews.setTextViewText(R.id.msg,"chapter_5");**//更新TextView
	** remoteViews.setImageViewresource(R.id.icon,R.drawable.icon1);  **

	PendingIntent pendingIntent2 = PendingIntent.getActivity(this,0,new intent(this,Activity2.class）,PendingIntent.FLAG_UPDATE_CURRENT);
	notification.contentView = remoteViews;
	notification.contentIntent = pendingIntent;
	NotificationManager manager =(NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
	manager.notify(2,notification);
	
###4、桌面小部件的实现：(SystemServer)
	桌面小部件是通过AppWidgetProvider（本质是一种广播）的方式来实现。
	
	桌面小部件最常用的方法有：onUpdate,onEnabled,onDisabled,onDeleted,这几个方法会在onReceive中的合适的时间来被调用。
	onEnabled:当桌面小部件第一次添加到桌面的时候被调用，可以添加多次，但是只在第一次调用。

	onUpdate:小部件被添加，或者是每次被刷新的时候调用

	onDeleted:每删除一次桌面小部件的时候就调用一次

	onReceive:用于分发具体的事件给其他方法。

###5、PendingIntent:

## ①PendingIntent和Intent的区别在于： ##

	PendingIntent是使用场景给RemoteViews添加单击事件，因为RemoteView是运行在远程进程的，因此RemoteViews不同于普通的View,所以无法直接像View那样通过setOnclickListener的方式来设置单击事件。通过send和cancel的方式来发送和取消。

	Intent则是表示即将发生的事情

	②PendingIntent的匹配规则：如果两个PendingIntent的内部的Intent相同，并且RequestCode也相同，那么这两个PendingIntent就是相同的PendingIntent.

	Intent的匹配规则是：Intent的ComponentName和intent-filter都相同，那么这两个Intent才是相同的。

	③PendingIntent的flag参数：
	FLAG_ONE_SHOT:PendingIntent只能被使用一次，然后就自动cancel.

	FLAG_NO_CREATE:PendingIntent不会主动创建，没有太多意义。

	FLAG_CANCEL_CURRENT:如果PendingIntent已经存在，那么它就会被cancel,然后系统会创建一个新的PendingIntent.

	FLAG_UODATE_CURRENT:如果当前描述的PendingInent已经存在，那么他们都会被更新，即他们的Intent中的Extras会被喜欢丞最新的。
	
###6、RemoteViews的意义：

- apply和reApply的区别：apply会加载布局并更新界面。reApply只会更新界面。
 
	比如现在有两个应用，一个应用需要能够更新另外一个应用的某个界面，这个时候一般会选择AIDL去实现，但是如果对界面的更新比较频繁，这个时候就会有效率问题。采用RemoteViews就没有这个问题了，但是其仅支持一些常见的View,对于自定义的View,其是不支持的。

	