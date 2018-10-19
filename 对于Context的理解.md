## 一、什么是Context

- 访问当前应用的资源（getResources,getAssets)，启动其他组件（Activity,Service,Broadcast)以及得到各种服务（getSystemService)。
- Context作为一个抽象的基类,其最终的实现是在ContextImpl中，Activity和Service，Broadcast是Context的子类。

## 二、Application,Activity,Service与Context的关系

##### 1、Context的真正实现是在ContextImpl中，也就是说Context的大部分方法调用都会转到ContextImpl中，而Activity,Application,Service的创建过程都是在ActivityThread中完成，activity的启动核心是在ActivityThread中完成的。

- Activity 继承自ContextThemeWrapper,有界面的概念。在ActivityThead中会new ContextImpl,并attach.

- Application 继承自ContextWrapper,没有界面的概念。所以没有Theme,在ActivityThead中会new ContextImpl

- Service 继承自ContextWrapper,没有界面的概念，所以没有Theme.

  

##### 2、Dialog的使用需要Activity,在桌面上采用Application的Context无法弹出对话框。当启动一个activity的时候，如果要一个Application的Context,需要设置标志位：FLAG_ACTIVITY_NEW_TASK ，否则无法启动。

##### 3、获取资源文件的时候，用哪个context都可以。

- 由于getResourse返回的是mResourse,其中管理resourse的是ResourcesManager（单例模式）,这样就保证了不同的ContextImpl访问的是同一套资源。

## 三、getApplication和getApplicationContext的区别

- getApplication返回的结果为Application,且不同的Activity和Service返回的Application均为同一个全局对象，在ActivityThread内部有一个列表专门用于维护所有应用的application。一个应用只有一个包信息。
- getApplicationContext : 返回的结果为Application对象，只不过返回类型为Context。
- 一个应用的Context的数量 = Activity的个数 + Service的个数 + 1，这个1为Application 