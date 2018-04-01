##2017/12/12:

#一、Dialog,Toast和Snackbar
1、Dialog:当提示信息至关重要的时候，并且需要由用户做出决定才能继续的时候使用。

2、Toast:当提示消息只是告知用户某个事情发生了，用户不需要对这个事情做出响应的时候。

3、以上两者之外的任何其他场景。

#二、为优化Toast等，一般需要封装Toast:这里使用开源的一个封装（SmartToast):
1、SmartToast的特点：

	  ①全局始终使用一个Toast实例，节省内存开销
	  
	  ②如果Toast正在显示，多次触发同一个内容的Toast,不会重复弹出。
	  
	  ③新的Toast(内容或位置发生了变化)来临时，会立即弹出，不会等到当前显示的Toast的duration耗尽再弹出，虽不会创建新的Toast实例，但具有切换效果（与你手机系统原生Toast的切换动画一致）
	
	  ④可修改Toast默认布局的风格，如背景颜色，文字大小和颜色等
	
	  ⑤可为Toast设置自定义布局，并进行代码处理

2、实现的demo:
	
	a.在Application中初始化SmartToast:

		SmartToast.plainToast(this);

	b.可以设置SmartToast的颜色样式等：

	        SmartToast.plainToast(this)
	                .backgroundColorRes(R.color.colorPrimary)
	                .textColorRes(R.color.colorAccent)
	                .textSizeSp(18)
	                .textBold(true)
	                .processPlainView(new ProcessViewCallback() {
	                    @Override
	                    public void processPlainView(LinearLayout outParent, TextView msgView) {
	                        super.processPlainView(outParent, msgView);
	                    }
	                });
	     }

	c.在Application中声明自己的Application:

		android:name=".MyApplication"

	d.在Activity中使用：

		 case R.id.btn_toast2:
                SmartToast.show("Dammyouh");
                SmartToast.showAtTop("Dammyouh");
                SmartToast.showInCenter("Dammyouh");



##2017/12/13:
###一、JSONObject和JSONArray的使用：

1、Json里面的数据是以一种键值对的方式存在（"key","value"),其语法多是{}，[]的单独形式或者组合形式。

2、对于JsonObject:

	a.用{}包含一些列无序的Key_Value键值对表示，key与Value之间用冒号分隔，每个Key-Value之间用逗号分隔.

	b.对于纯JsonObject的解析：

	 String json="{'name':'林书豪','age':'24'}";
        try{
            JSONObject obj = new JSONObject(json);
            String name = obj.getString("name");
            int age = obj.getInt("age");

            System.out.print(name+" "+age);
        }catch (JSONException e){
            e.printStackTrace();
        }

3、对于JSONArray:

	a.用[]包含的数组

    b.对于纯JsonArray的解析：

	 String json="['天津冷','北京暖’,'东京热’,'南京凉']";

        JSONArray jArray = new JSONArray();
        int length = jArray.length();
        for(int i = 0;i < length; i++){
            String string = jArray.getString(i);
            System.out.print(string+"");
        }


4、对于组合类型的解析：则需要两者结合。
	http://blog.csdn.net/miaozhenzhong/article/details/52585726


##2017/12/14

###一、对于EventBus的再次理解：
	①EventBus.getDefault().postSticky(new UnReadNotifyEvent(totalCount_notify));
	如果在同一个模块中，存在覆盖。
	②在某个地方发送了通知事件的通知之后，只有在同一个模块的才能接收到这个通知。
###二、对于Git的深入使用总结：

1、一般在项目开发的时候，先从总分支上切一个分支下来，若两人合作开发某个模块，则每个人从切下来的分支再切一个分支下来做修改，当修改完后push到第一次切下来的分支，最后由一个人将第一次切下来的分支push到总分支上，完成某个功能的开发。

2、上述操作的常用命令：

 	①.查看自己关联的远程分支：
	git remote -v

	②.如果没有要对应的远程分支，则使用以下命令添加一个远程分支：
	git remote add 可指代远程仓库的名字 地址：
	git remote add pb git://github.com/paulboone/ticgit.git

	③通过pb可以指代对应的仓库地址，用以下命令抓取远程仓库的代码：
	git fetch pb

	④切换到本地做修改的分支，做修改后，切换到这个与远程的对应的分支，进行merge.合并后再git pull 到远程。

###三、网络请求中的注意事项：

1、在网络请求的调试过程中，不要单步运行，因为这是一个异步才做，单步调试不能走到请求里面，需要设置多个断点，进行调试。


###四、IntentService的使用：

####1、为什么要用IntentService:
	Service 的回调方法（onCreate,onStartCommand,onBind,onDestroy)都是运行在主线程的，通过startService启动Service之后，就需要在onStartCommand方法中完成工作，当执行一些耗时操作时，就会出现ANR问题。所以为解决这个问题，采用IntentService.

####2、IntentService的特点：
	①IntentService自带一个线程，当Service需要做一些可能会阻塞主线程的工作时，就考虑使用IntentService.

	②需要做的实际工作是放在IntentService中的onHandleIntent(Intent intent)方法中，运用这里传入的intent去做实际的操作，onHandleIntent执行在IntentService所持有的工作线程中，不是主线程。

	③当startService多次启动了IntentService,就会产生多个Job.IntentService仅仅持有一个工作线程，所以只能一个一个的去处理，这些任务是以队列的形式去执行。

####3、对于IntentService的使用：
	①新建一个Java.class继承IntentService,实现其中需要重写的方法，特别是onHandleIntent(Intent intent).
    ②通过Intent中的携带的参数，去执行某些耗时操作。
    ③在Activity或者Fragment中去存入执行耗时操作时需要用的值。


##2017/12/18
一、HashMap的遍历：

###1、第一种方式：（效率比较高：只遍历了一次，把key和value都放在了entry中)
	①HashMap map = new HashMap();
	Iterator iter = map.entrySet().iterator();
	while(iter.hasNext()){
	Map.Entry entry = (Map.Entry)iter.next();
	Object key = entry.getKey();
	Object val = entry.getValue();
    }

###2、第二种方式：(效率较低：keySet其实是遍历了2遍，一次转换成iterator,一次从hasgmap中取出Key所对于的value.
	Map map = new HashMap();
	Iterator iter = map.keySet().itertor();
	while(iter.hasNext()){
	Object key = iter.next();
	Object val = map.get(key);
	}

	
##2017/12/19

###一、foreach的使用：

1、foreach 是java5的新特性之一，在遍历数组，集合方面有很大用处。foreach不是一个关键词，而是把增强型的for语句称为foreach语句。

2、结构是：
	for(part1 : part2){
	part3
	}
	
	例如：
	for (NotifyEntity entity : mUnreadNoticeList) {
        counts = counts + entity.getNotifyCount();
                        }

###二、greenDao的对于表的操作总结：(需要将App卸载重装，才能走建表语句）

1、一般要写一个DaoUtil,用于获取对表操作的对象：
	在DaoUtil中主要代码：

	DaoMaster.OpenHelper helper = new ......
	DaoMaster daoMaster = new DaoMaster(helper.getWritableDatabase());
2、在使用的时候是先得到一个daoSession对象，再进行对于数据库中表的操作：
	DaoSession daoSession = DaoUtil.getDaoSession(context);

3、对表进行执行增删改查：

增加：

	①.例如增加：（insertOrReplaceInTx)：使用事务操作，将给定的实体集合插入数据库，所此实体存在，则覆盖：
	daoSession.getNotifyEntityDao().insertOrReplaceInTx(mUnreadNoticeList);
	
    ②insertInTx:将给定的实体插入数据库
	③insertorReplace(T entity):将给定的实体插入数据库，若此实体类存在，则覆盖。
    ④save(T entity):将给定的实体插入数据库，若实体存在，则更新。
删除：

	delete(T entity)：从数据库中删除给定的实体  
	deleteAll() ：删除数据库中全部数据  
	deleteByKey(K key)：从数据库中删除给定Key所对应的实体  
	deleteByKeyInTx(java.lang.Iterable<K> keys)：使用事务操作删除数据库中给定的所有key所对应的实体  
	deleteByKeyInTx(K... keys)：使用事务操作删除数据库中给定的所有key所对应的实体  
	deleteInTx(java.lang.Iterable<T> entities)：使用事务操作删除数据库中给定实体集合中的实体  
	deleteInTx(T... entities)：使用事务操作删除数据库中给定的实体  

修改：

	update(T entity) ：更新给定的实体  
	updateInsideSynchronized(T entity, DatabaseStatement stmt, boolean lock)   
	updateInsideSynchronized(T entity, android.database.sqlite.SQLiteStatement stmt, boolean lock)   
	updateInTx(java.lang.Iterable<T> entities) ：使用事务操作，更新给定的实体  
	updateInTx(T... entities)：使用事务操作，更新给定的实体 

查询：使用QueryBuilder自定义查询实体，而不是再写繁琐的SQL语句，避免SQL语句出错率。

	官方给的查询例子：

	①List joes = userDao.queryBuilder()  
                   // 查询的条件  
                   .where(Properties.FirstName.eq("Joe"))  
                   // 返回实体集合升序排列  
                   .orderAsc(Properties.LastName)  
                   .list();  

	②QueryBuilder qb = userDao.queryBuilder();  
	// 查询的条件  
	qb.where(Properties.FirstName.eq("Joe"),  
	qb.or(Properties.YearOfBirth.gt(1970),  
	qb.and(Properties.YearOfBirth.eq(1970), Properties.MonthOfBirth.ge(10))));  
	List youngJoes = qb.list(); 
	
	③其他为分页等不方便而存在的api:
	limit(int)：限制查询返回结果的数目  
	offset(int)：设置查询结果的偏移量，此查询需与limit(int)结合使用，而不能够脱离limit(int)单独使用 


 	④如果查询的结果是多个（返回的结果是一个集合等）
	list()：所有实体加载至内存，结果通常是一个ArrayList  
	listLazy()：实体在需要时，加载至内存，表中的第一个元素被第一次访问时会被缓存，下次访问时，使用缓存  
	listLazyUncached()：任何对列表实体的访问懂事从数据库中加载  
	listIterator()：以按需加载的方式来遍历结果，数据没有被缓存
 
	List<NotifyEntity> mUnreadNoticeListFromDB = daoSession.getNotifyEntityDao().queryBuilder().list();

###三、Gson的使用方法：
	1、new Gson().fromJson(String str)会尽量转换出一个对象，
	如果长度为0或者null,则返回的结果是null.
	如果非空，但不是Json格式，则会抛出异常

###四、Glide4的使用：

####基本用法：

1、在app/build.gradle中添加如下依赖：

	dependencies {
    implementation 'com.github.bumptech.glide:glide:4.4.0'
    annotationProcessor 'com.github.bumptech.glide:compiler:4.4.0'
	}

2、在权限声明中需要添加网络权限：

	<uses-permission android:name="android.permission.INTERNET" />

3.加载图片：（先With,再load,再into)

	 String url = "http://guolin.tech/book.png"; 
        Glide.with(this).load(url).into(imageView); 

####特殊用法：
	1、占位图：(在使用的时候很可能看不到占位图，因为Glide有非常强大的缓存机制，下载过的图片Glide会自动把它缓存下来，所以加载的时候直接读取，比较快）同时可以指定其图片大小。
	RequestOptions options = new RequestOptions() 
        .placeholder(R.drawable.ic_launcher_background) 
        .error(R.drawable.error) //表示error时的占位图
        .diskCacheStrategy(DiskCacheStrategy.NONE); 
	Glide.with(this) 
     .load(url) 
     .apply(options) 
     .into(imageView);

	2、Glide的缓存分为两种形式：
	内存缓存：防止应用重复将图片数据读取到内存中
	硬盘缓存：防止应用重复从网络或者其他地方重复下载和读取数据

    3、可指定加载格式：（gif等支持）
	Glide.with(this) 
     .load("http://guolin.tech/test.gif") 
     .into(imageView);
	



##2017/12/21
1、将原APP在首页和我的页面的找老师入口更改为通知类消息入口，并根据需求尺寸进行设置，添加其交互，点击进入通知消息列表页

实现方法：在进行交互的时候由于首页属于bbs模块，我的页面属于UserCenter模块，所以用的是ARouter的路由跳转方式。

2、在更改了通知入口的基础上，添加通知未读总数的显示，并且在首页和我的页面的显示一致，实现实时刷新。
实现方法：①根据UI的设计进行布局设置，写了一个请求未读总数接口工具类，通过ARouter中的服务的调用形式，实现不同模块的来调用这个工具类，在Framegment回调onResume的时候通过这个工具类来请求后台，刷新未读数的总数。

②在XiaomiPush的通知类消息到来的时候，也需要请求接口，实现实时刷新。

③当请求到后台未读总数的接口的时候，后台返回的时一个Json字符串,这里用的是JsonArray的解析方式，将其解析并存入本地数据库，方便枭哥的通知列表页直接从数据库取值，并避免再次请求相同的接口。

3、将原APP的Message模块的消息列表页面的通知类（系统提醒，评论回复等）移除，修改这个页面的找老师入口的样式
实现方式：删除与通知相关的布局文件和代码，根据UI的设计样式，微调其尺寸等
	

4、实现原来APP的用户群在一般情况下也可实现左划删除该群聊（原来只有在自己被提出群聊的时候才能左划删除），在次有群消息的时候显示该群聊
实现方式：修改原来运用左划删除的逻辑，将原来的对于一般情况的限制去除

5、找老师入口引导图的显示
实现方式：①根据UI的设计，将该图的展示是先将页面等分成2个LinearLayout，使得引导图处于消息的正上方，但是还是存在偏差，最后通过weight的慢慢调整实现其位置的确定。s

②在显示逻辑上，当用户卸载App时，再重装时，根据发帖的引导图关闭后再显示该引导图。 这里在SharePreference中设置了是否显示该引导图的TAG，显示后将其设置为False,再次登陆的时候就不再显示。

6、XiaomiPush中信息通道的跳转，根据请求接口的mLink,进行跳转
实现方式：①后台返回的mLink是包含Android与Ios需要用的信息，写了一个工具类，将这个字符串各部分需要的信息进行分离，并且按照Key-Value的形式存入HashMap.

②点击XiaomiPush要实现跳转，在这个工具类中，封装了需要根据参数个数进行传参的ARouter跳转,是通过HashMap迭代器的方式，实现key—value的遍历，并传入.WithString中。

7、已读清零的接口封装，当已读的时候调用该类：
实现方式：这个类的虽然都是对于后台接口请求封装，但是这里采用的是继承自IntentService的形式，在其HandIntent()中实现真正的请求，这样做的好处见该项目下的思考。

8、在社区互动中根据不同的类型传递不同的ID跳转到不同的页面
实现方式：在MoudleIntent中封装ARouter的跳转方式，并实体类中后台返回的结果传递参数。

思考：
1、在实现清零的方式上：
本是想着用传Context的方式来进行不同地方的请求清零，但是请求的时候用到的是一个匿名内部类，该Context就是final的，如果该 final型的Context在Activity中使用的时候，context没使用完，就切换界面，则那个Activity不会走OnDestroy(),即Activity没有释放，容易造成内存泄漏。

2、在社区互动需要建表遇到的问题：
在建表的时候，尽量不要使用后台返回的数字型参量作为主键，而应该自己建一个主见自增列。因为实际上在建表的过程中，会根据主键生成一个_id的列，该列的类型为Long,所以在设置主键的时候应该设置一个为Long的主键，这样才不会发生强制类型转换的错误。其次在解析后台返回的Json时，根据其返回的数据类型选择解析方式（JsonArray还是JsonObject)

3、在根据社区互动的类型实现跳转的情况区分：
由于这一块的类型比较多，后台返回的参数比较多，所以需要细心和耐心，现在的总结是1-3是关于帖子的回答(评论了你的帖子，回复了你的评论，评论了你的评论），4-7是对于问题的回答（回答了你的问题，评论了你的回答，回答了你的评论，邀你回答问题），再根据情况区分其后台返回的各个类型的相关参数，在调试的时候也遇到一些问题，最后通过对比的方式，找出Android与ios的哪些调试的结果不一样，最后得到更正。

4、


发现的不足：
1、在代码上面，由于自己对于设计思想，设计模式不是很懂，在看代码的时候有部分比较吃力，所以在写代码的时候也不是特别正规。

2、在解决问题的时候，习惯于问，但是有的只要多看看就可以完成，所以在自主查看问题方面还要加强。

3、在有需求的时候，有时候不能特别合理的安排时间。比如有了一个问题，总是一直看这个问题，如果还是没有结果，则导致其他的需求也没法完成，所以，在合理安排时间上需要改善。

4、在忙的时候，不写禅道上的记录，导致一周下来，对于自己所做的任务不是很清楚，所以还是要养成日志记录的习惯。


思考：
1、合理安排时间的基础上，自学是必不可少的，每天应当安排一些时间去看看需求外的知识点。

2、在做需求之前，不要急着写代码，而应该是先预览，做好规划，可以避免需求落下。

3、在写代码后一般都要进行自测，对于自己的问题，或者是自测中发现的以前的问题，需要问清楚，或者考虑自己产生的问题，对于其他模块是否有影响。



对于公司的期望：
1、希望公司在网络多配几个路由，有时候网太差，对于请求接口，刷新数据等都比较不方便。

2、厕所有点少，每次去感觉都要等，有点浪费时间额。

3、希望有一个会议室的借用管理系统，避免借用会议室的冲突。
	
对自己的期望：
1、在写代码之前，认真查看需求文档，争取做到不落下需求。
2、希望自己在效率上面有所提高，为队友分担更多的任务。
3、在工作中遇到的问题，有价值的要记录。
	
