##2017/12/7
###一、URI和URL的区别：
1、URI：统一资源标识符，用来唯一的识别一个资源，包含URL和URN。Uri时Android开发的，扩展了JAVA的URI的一些功能来特定的适用于Android开发。

2、URL：统一资源定位符，是一种具体的URI，也就是URL可以用来识别一个资源，还指明了如何定位一个资源。

###二、URI的使用：
1、基本形式：http://www.baidu.com/artical.....
	
	[scheme:][//authority][path][?query][#fragment] 
	①path:可以有多个，每个用/连接；
	②query参数可以带有对应的值，用&连接，也可以不带
	③ 在Android中，schema,authority是必须要有的，但是parh,query等是选择性的。

2、终极划分：

	[scheme:][//host:port][path][?query][#fragment]  

3、举例：

	http://www.java2s.com:8080/yourpath/fileName.htm?stove=10&path=32&id=4#harvic  
	
	①scheme:http
	②host:www.java2s.com
	③port:8080
	④query:是在？后面的部分的键值对：stove=10&path=32&id=4
	⑤authority:包含host和port:www.java2s.com:8080
	⑥fragment:在最后用#分隔的部分,harvic

4、代码的提取：

	http://www.java2s.com:8080/yourpath/fileName.htm?stove=10&path=32&id=4#harvic
	
	①getScheme:获取Uri中的Scheme字符串的部分，即http
	②getSchemeSpecificPart:获取Uri中的scheme-specific-part部分，即//www.java2s.com:8080/yourpath/fileName.htm?
	③getAuthority:获取Uri中的Authority中的部分，即www.java2s.com:8080
	④getQuery:获取Uri中的query的部分：即stove=10&path=32&id=4
	⑤getHost:获取Authority中的host部分，即www.java2s.com
	⑥getPort:获取Authoruty中的Port部分，即8080
	⑦还有两个特别常用的：List<String> getPathSegments():
	
	String mUriStr = "http://www.java2s.com:8080/yourpath/fileName.htm?stove=10&path=32&id=4#harvic";  
	Uri mUri = Uri.parse(mUriStr);  
	List<String> pathSegList = mUri.getPathSegments();  
	for (String pathItem:pathSegList){  
	    Log.d("qijian","pathSegItem:"+pathItem);  
	} 
	以上分别打印出yourpath和fileName.htm
	
	⑧getQueryParameter(String key):在上面我们通过getQuery()获取整个query字段：stove=10&path=32&id=4，getQueryParameter(String key)作用就是通过传进去path中某个Key的字符串，返回他对应的值。

5、使用getQueryParameterNames(),将键值对存入hashMap:

	 public static Map<String, String> getAndroidParamsByLinkUrl(String url) {
	    Map<String, String> params = new HashMap<>();
	    if (isLegallUrl(url)) {
	        String androidRouterUrl = extractAndroidRouterUrl(url);
	
	        if (!TextUtils.isEmpty(androidRouterUrl)) {
	
	            Uri uri = Uri.parse(FAKE_HEADER + androidRouterUrl);
	
	            for (String item : uri.getQueryParameterNames()) {
	                params.put(item, uri.getQueryParameter(item));
	            }
	        }
	
	    }
	    return params;
	}	


###三、判断一个输入的字符串是否为整数（使用正则表达式）：

 	public static boolean isInteger(String str) {
        Pattern pattern = Pattern.compile("^[-\\+]?[\\d]*$");
        return pattern.matcher(str).matches();
    }



##2017/12/28
###一、OKhttp的自定义缓存的实现：
1、网络请求框架的缓存基本实现：

	有缓存用缓存的数据，没缓存发起http请求取数据，得到最新的数据后存到缓存里。

2、OKhttp请求的源码实现：

	①在execute中实现中调用gerResponseWithInterceptorChain，这个方法是初始化一个interceptor列表，然后调用RealInterceptorChain的proceed函数。
3、interceptors(拦截器的作用）：

	①interceptor的add顺序很重要
	②只要Interceptor的intercept方法调用了chain.proceed(request),就会调用Interceptor列表里的下一个Interceptor;反之可以不调用chain.proceed代码里，走下一个Interceptor的流程。
	③自定义的application interceptor和network interceptor时，都必须返回chain,proceed得到的结果；否则会打断okhttp内部的请求链。
	④在写application interceptor时，在调用chain.proceed(request)之前包装request.
	⑤写network interceptor时，在调用chain.proceed(request)之后得到的response包装response.

##2017/12/29
###一、RecyclerView的再次总结：
	1、在使用RecyclerView的时候，布局文件中肯定要有这个组件。
	2、Recyclerview的Adapter的写法，继承自RecyclerView.Adapter,在里面实现构造函数，onCreateViewHolder,onBindViewHolder,getItemCount,getItemViewType等函数。
	3、注意写ViewHolder类，在ViewHolder中绑定组件，这个类一般写成静态类。
	4、可以根据viewType来实现一个页面的每一行的不同展示，viewType可以作为区分然后New出不同的ViewHolder。

###二、RecyclerView的ItemDecoration的使用：
	1、在itemDecoration中的方法：
		onDraw(),onDrawOver,getItemOffsets






