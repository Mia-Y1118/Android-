## bugly的bug总结：

### 1、DialogFragment的出现的问题：

##### ①DialogFragment的状态丢失问题：

由于在showDialogFragment的时候没有判断Activity的状态，导致宿主Activity销毁之后还在弹DialogFragment,最终出现Crash. 

```java
if(context!=null  && !context.isFinishing() && !context.isDestroyed()){
     showResignDialog(entity);
} 
```

##### ②在DialogFragment弹出的时候，最好判断一下该DialogFragment是否已经被Add.

```java
if(!mDialog.isAdded()){
    mDialog.show(。。。。。);
}
```

2、注意空指针异常，做好防御性编程

##### 3、



