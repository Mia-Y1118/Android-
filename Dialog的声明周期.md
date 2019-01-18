一、Dialog的声明周期一共有6个，onCreate,show,onStart,cancel,onDismiss,Stop

```kotlin
1、Dialog仅在第一次启动时候会执行onCreate(),之后无论该Dialog执行Dismiss()，cancel(),stop(),Dialog都不会再执行onCreate()方法。

2、show()和onStart()在每次Dialog显示的时候都会依次执行。

3、onDissmiss()和stop()在每次Dialog消失的时候都会依次执行。

4、cancel()是在点击BACK按钮或者Dialog外部时触发，然后依次执行onDismiss()和stop().
```

```javascript
生命周期的场景执行：

①、点击显示按钮，第一次显示Dialog,然后按BACK键返回：

show() --> onCreate() -->onStart() -->BACK -->cancel() --> onDissmiss() -->Stop()

②、再次点击显示按钮，然后点击Dialog外部 ：

show() —> onStart();  cancel() —> onDismiss() —> Stop(); 

③、再次点击显示按钮，然后执行Dialog.dismiss() 方法。 

show() —> onStart(); onDismiss() —> Stop();

```

二、Dialog的dismiss和cancel的区别：

```javascript
①dismiss : 作用只是dismissDialog（）；
②cancel：回调setOnCancelListener的监听事件—–>执行dismiss
```



