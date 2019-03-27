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

 

异常5：

### [java.lang.IllegalStateException](https://bugly.qq.com/v2/crash-reporting/crashes/1104528778/151596?pid=1) Fragment GroupWorkFragment{ce55c91} not attached to a context. 

原因：由于Fragment还没有Attach到Activity时，调用了如getResourses().getString(R.string.)

解决方案：在getResources之前添加一个isAdded的判断。



异常6： 

#### java.lang.IllegalStateException：Can not perform this action after onSaveInstanceState

原因：在关闭dialogFragment的时候，有可能dialogFragment已经执行了onSaveInstanceState.

解决方案：在关闭dialogFragment的时候，尽量使用dissmissAllowingStateLoss.



异常7：

#### Attempt to invoke virtual method 'void androidx.fragment.app.Fragment.setMenuVisibility(boolean)' on a null object reference

原因：在Fragment的Adapter中，getItem如果返回了空的Fragment,就会报这个错误。



### 2、注意空指针异常，做好防御性编程

### 3、Kotlin的相关bug

- 在使用Adapter,Dialog等，在findId的时候需要通过ItemView来find。
- 在使用DialogFragment的时候，最外层布局的宽度一定要match_parent,并且最外层设置为透明色，并且设置Gravity为center..
- 注意在使用Kotlin的时候，在使用从Intent中获取数据的时候，从而命名变量的时候需要考虑为空的情况。 

### 4、RecyclerView的notifyDataSetChanged()

- 在List指向的不是一个引用的时候不是一个使用notifysetchanged没用。
- 为防止RecyclerView的notifyDataSetChanged()没用的情况，最好在adapter中使用notifyDataSetChanged()。
- 在数据源发生改变的时候，需要notifyDataSetChanged

