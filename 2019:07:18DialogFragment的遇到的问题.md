#### 一、在判断dialogFragment的isAdd后有可能还是会报 Fragment already added的illegalStateException.

```java
原因：dialogFragment的显示是通过FragmentTransaction,在执行show DialogFragment的时候，其实系统就是创建了一个Runnable对象，然后通过Handler.post方式，将消息发送到消息队列，等待执行。如果用户多次连续点击的时候，可能发送到消息里的消息尚未执行到，但是新的点击事件已经发送过来，如果仅仅通过DialogFragment.isAdded()进行判断，有可能得到错误的结果。

//修改方式：在判断isAdded之前先执行任务
 act?.supportFragmentManager?.executePendingTransactions()
 if (punchDialog?.isAdded == false) {
     punchDialog?.show(act?.supportFragmentManager, "punchDialog")
 }
//2层保险：在show的时候，对上一次的dialogFragment进行移除
override fun show(manager: FragmentManager?, tag: String?) {
  try {
      //在每个add事务前增加一个remove事务，防止连续的add
      manager?.beginTransaction()?.remove(this)?.commitAllowingStateLoss()
      super.show(manager, tag)
  } catch (ex: Exception) {
      ex.printStackTrace()
  }
}

```

