#一、Bugly的集成：

- 首先在bugly的官网申请一个账号，并创建一个自己的产品,创建之后可以看到应用的APPID。
- 一般选择自动载入的方式：

		1、先在app的build.gradle中添加最新版的bugly SDK版本号的依赖
		 compile 'com.tencent.bugly:crashreport:latest.release' //其中latest.release指代最新Bugly SDK版本号，也可以指定明确的版本号，例如2.2.0

		2、同时集成sdk和ndk:

		android {
   		 defaultConfig {
        	ndk {
            // 设置支持的SO库架构
            abiFilters 'armeabi' //, 'x86', 'armeabi-v7a', 'x86_64', 'arm64-v8a'
       			 }
    		}
		}

		dependencies {
    		compile 'com.tencent.bugly:crashreport:latest.release' //其中latest.release指代最新Bugly SDK版本号，也可以指定明确的版本号，例如2.1.9
    		compile 'com.tencent.bugly:nativecrashreport:latest.release' //其中latest.release指代最新Bugly NDK版本号，也可以指定明确的版本号，例如3.0
		}

- 在AndroidManifest中添加权限：

		<uses-permission android:name="android.permission.READ_PHONE_STATE" />
		<uses-permission android:name="android.permission.INTERNET" />
		<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
		<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
		<uses-permission android:name="android.permission.READ_LOGS" />
- 添加混淆

		-dontwarn com.tencent.bugly.**
		-keep public class com.tencent.bugly.**{*;}

- 在Application中初始化：
   
           private void initBugly() {
            CrashReport.UserStrategy strategy = new CrashReport.UserStrategy(this);
            strategy.setAppChannel(Utils.getChannel(this));
            strategy.setAppPackageName(getPackageName());
            strategy.setAppVersion(AppInstance.VERSION_NAME);
            if ("release".equals(AppInstance.BUILD_TYPE)) {
                CrashReport.initCrashReport(getApplicationContext(), APPID, false, strategy);
            } else {
                CrashReport.initCrashReport(getApplicationContext(), "b92fea1379", false, strategy);
            }
            CrashReport.setUserId("1995");
  		  }

#二、MTA的集成：
- 有自动集成（jcenter)和手动接入的方式，一般选择jcenter的方式
- 首先完成注册，并申请一个账号，有APP ID 和APP KEY；
- 在app的build.gradle中导入依赖：
		defaultConfig {
		applicationId "你的包名"...}

		ndk {
		//根据需要 自行选择添加的对应cpu类型的.so库。
		abiFilters 'armeabi', 'armeabi-v7a', 'arm64-v8a'
		// 还可以添加 'x86', 'x86_64', 'mips', 'mips64'
		}

		manifestPlaceholders = [
		MTA_APPKEY:"注册应用的appkey",
		MTA_CHANNEL:"渠道名称"
		]



		// mta包稳定版和测试版本二选一，mid包必须要添加 ，可视化埋点根据需要添加

		//mta 3.3 稳定版
		compile 'com.qq.mta:mta:3.3.1-release'
		//mid  jar包 必须添加
		compile 'com.tencent.mid:mid:3.73-release'
		//可视化埋点的相关jar包 （根据需要添加），可视化埋点的版本号，和必须和当前MTA的版本号必须匹配使用 需要在配置文件中增加配置，具体请参考 高级功能中可视化埋点的接入。
		compile 'com.qq.visual:visual:3.3.1-release'
		
		
		//MTA 3.4.0 beta 版本
		
		compile 'com.qq.mta:mta:3.4.0.1-beta'
		
		compile 'com.tencent.mid:mid:3.73-release'
		
		//可视化埋点的相关jar包 （根据需要添加），可视化埋点的版本号，和MTA的版本号必须匹配，只能是当前版本号成对存在。且可视化埋点需要依赖MTA
		compile 'com.qq.visual:visual:3.4.0-beta'

- 添加代码混淆：
		
		-keep class com.tencent.stat.* { ;}
		-keep class com.tencent.mid.* { ;}

- 在Application中的onCreate()中进行设置：

		private void initBugly() {
            CrashReport.UserStrategy strategy = new CrashReport.UserStrategy(this);
            strategy.setAppChannel(Utils.getChannel(this));
            strategy.setAppPackageName(getPackageName());
            strategy.setAppVersion(AppInstance.VERSION_NAME);
            if ("release".equals(AppInstance.BUILD_TYPE)) {
                CrashReport.initCrashReport(getApplicationContext(), "b92fea1379", false, strategy);
            } else {
                CrashReport.initCrashReport(getApplicationContext(), "b92fea1379", false, strategy);
            }
            CrashReport.setUserId("1995");
    }