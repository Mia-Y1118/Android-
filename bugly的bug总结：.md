## bugly的bug总结：

### 1、DialogFragment的出现的问题：

异常1：

Java.lang.IllegalStateException: Can not perform this action after onSaveInstanceState 

原因：在Add Fragment的时候，宿主Activity已经调用了onSaveInstanceState,此时宿主Activity已经退到了后台，onSaveInstanceState之后Add Fragment,Fragment没有机会再保存状态，回收了没有数据进行恢复。

解决方案：

##### ①判断宿主Activity的状态，以免DialogFragment的状态丢失问题：

由于在showDialogFragment的时候没有判断Activity的状态，导致宿主Activity销毁之后还在弹DialogFragment,最终出现Crash. 

```java
if(context!=null  && !context.isFinishing() && !mDialog.isAdded()){
     mDialog.show(。。。。。);
} 
```

##### ②在Add Fragment的时候，使用commitAllowingStateLoss,允许Fragment状态丢失：

```java
 FragmentManager fm = fragmentActivity.getSupportFragmentManager();
        FragmentTransaction ft = fm.beginTransaction();
        ft.add(this, this.getClass().getSimpleName());
        ft.commitAllowingStateLoss();
```



异常2：

java.lang.IllegalStateException: Fragment already added

原因：

当快速双击调用FragmentTansaction.add()方法添加fragmentA，而fragmentA不是每次单独生成的，就会引起这个异常。

解决方案：在Add的时候，先判断fragmentA.isAdded(),避免发生该异常

```java
if(!fragmentA.isAdded()){ FragmentManager manager = ((FragmentActivity)context).getSupportFragmentManager(); FragmentTransaction ft = manager.beginTransaction(); ft.add(fragmentA, "fragment_name"); ft.commit(); } 
```

异常3：(暂时未遇到)

### java.lang.IllegalStateException: Fragment MyFragment{410f6060} not attached to Activity

原因：Fragment的还没有Attach到Activity时，调用了如getResource()等，需要上下文Content的函数。 

解决方案：

等将调用的代码写在OnStart（）中。另外一种解决方法主要是在调用：  getResources().getString(R.string.app_name); 之前增加一个判断isAdded(), 



异常4：（暂时未遇到）

### java.lang.IllegalStateException: Fragment already active

原因：

在 Fragment 没有被添加到 FragmentManager 之前，我们可以通过 Fragment.setArguments() 来设置参数，并在 Fragment 中，使用 getArguments() 来取得参数。在 Fragment 被添加到 FragmentManager 后，一旦被使用，我们再次调用 setArguments() 将会导致 java.lang.IllegalStateException: Fragment already active 异常。  

解决方案：

可以使用setter和getter方法进行数据的存储和获取。  

 



2、注意空指针异常，做好防御性编程

##### 3、



