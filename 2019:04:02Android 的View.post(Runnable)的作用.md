#### 一、View.post(Runnable)方法介绍：

```java
//在post(Runnableaction中)，View获得当前线程(即UI线程)的handler,然后将action对象post到Handler中，在Handler里，将传过来的action对象包装成一个message,然后将其投入UI线程的消息循环中。所以可以同通过这个方法实现在UI线程中更新UI。
viewPager.post(new Runnable() {
            @Override
            public void run() {
                viewPager.setCurrentItem(index, false);
            }
        });
```

