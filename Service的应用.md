#### 一、Service 和Activity是运行在主线程的，可能会处于不同的进程。

#### 二、启动方式：

- ##### startService

```java
//onCreate()只会在Service第一次创建的时候调用，如果当前Service已经被创建过了，不管怎么调用startService(),onCreate()方法都不会再执行，但是会执行onStartCommand()  

//onStart() 已经被废弃
onCreate() --> onStartCommand --> Running --> onDestroy()
```

- ##### bindService

```java
//onBind()是用来和Activity建立关联的。
onCreate() --> onBind() --> Running --> onDestroy()

//与Activity建立关联的Service
class MyService : Service() {
    
    private var mBinder : MyBinder = MyBinder()
    override fun onBind(intent: Intent?): IBinder? {
        return mBinder
    }
    
    class MyBinder : Binder(){
        fun startDo(){ 
        }
    }
}

class MyActivity : Activity() {
    private lateinit var serviceConnection : ServiceConnection 
    private var myBinder : MyService.MyBinder  ?= null
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        serviceConnection = object : ServiceConnection{
        	//在Activity与Service解除关联的时候调用
            override fun onServiceDisconnected(name: ComponentName?) {
            }
            
            //在Activity与Service建立关联的时候调用
            override fun onServiceConnected(name: ComponentName?, service: IBinder?) {
                myBinder = service as MyService.MyBinder
                myBinder?.startDo()
            }
        }
        val intent = Intent(this,MyService::class.java)
        bindService(intent,serviceConnection, Context.BIND_AUTO_CREATE)
    }
}
```

#### 三、Service的销毁（onDestroy)

- ##### 只有一个处于停止状态并且没有Activity与其建立关联的Service才会被销毁。



