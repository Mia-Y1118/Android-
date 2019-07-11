####   一、使用限制

- 一个页面上只能有一个跑马灯效果

  ```java
  //使用方法：加入以下属性
  	 					android:singleLine="true"
              android:ellipsize="marquee"
              android:focusableInTouchMode="true"
              android:focusable="true"
  ```

#### 二、若要是的多个TextView同时实现跑马灯效果，则需要重写isFocused方法

```java
  ① override fun isFocused(): Boolean {
        return true
    }

  ②再在布局文件中加入：
            android:singleLine="true"
            android:ellipsize="marquee"
            android:focusableInTouchMode="true"
            android:focusable="true"
              
  ③android:marqueeRepeatLimit="marquee_forever" 这个属性是设置无限重复，如果不设置的话，默认会在3次之后停止不动了            
```

